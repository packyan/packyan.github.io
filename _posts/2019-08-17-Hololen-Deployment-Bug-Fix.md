# hololens 部署出错



无法通过 Visual Studio 进行连接和部署到 HoloLens

 备注

上次更新:8/8 @ 5: 晚上11点-Visual Studio 已发布 VS 2019 版本 16.2, 其中包括对此问题的修复。 建议更新到此最新版本, 以避免出现此错误。

Visual Studio 发布了 VS 2019 版本 16.2, 其中包括对此问题的修复。 建议更新到此最新版本, 以避免出现此错误。

问题根本原因:使用 Visual Studio 2015 或早期版本的 Visual Studio 2017 的用户在其 HoloLens 上部署和调试应用程序, 然后使用同一 HoloLens 的最新版本的 Visual Studio 2017 或 Visual Studio 2019 将受到影响。 较新版本的 Visual Studio 部署了新版本的组件, 但较旧版本中的文件仍保留在设备上, 导致较新版本失败。 这会导致出现以下错误消息:DEP0100:请确保目标设备已启用开发人员模式。 由于错误 80004005, 无法获取开发人员许可证。

**解决方法**：

尽管此问题在 Visual Studio 2019 16.2 中已修复, 但选择保留在 Visual Studio 早期版本中的开发人员可以使用以下步骤解决此问题, 并帮助取消阻止部署和调试:

1. 打开 Visual Studio
2. 文件->     > 项目
3. Visual C#     -> Windows Desktop > 控制台应用 (.NET Framework)
4. 为项目指定一个名称 (例如     HoloLensDeploymentFix), 并确保框架至少设置为 .NET     Framework 4.5, 然后单击 "确定"。
5. 右键单击解决方案资源管理器中的 "引用" 节点, 然后添加以下引用 (单击 "浏览" 部分, 然后单击 "浏览     ..."按钮):
             复制
             C:\Program Files (x86)\Windows     Kits\10\bin\10.0.18362.0\x86\Microsoft.Tools.Deploy.dll
             C:\Program Files (x86)\Windows     Kits\10\bin\10.0.18362.0\x86\Microsoft.Tools.Connectivity.dll
             C:\Program Files (x86)\Windows Kits\10\bin\10.0.18362.0\x86\SirepInterop.dll
             
              备注
             如果尚未安装 10.0.18362.0, 请使用最新版本。
6. 右键单击 "解决方案资源管理器中的项目, 然后选择"     > 现有项 "。
7. 浏览到     C:\Program Files (x86) \Windows Kits\10\bin\10.0.18362.0\x86, 并将筛选器更改为 "所有*文件*(.)"
8. 选中     "SirepClient" 和 "SshClient", 然后单击 "添加"。
9. 在解决方案资源管理器中找到两个文件 (这些文件应位于文件列表的底部), 并将属性窗口中的 "复制到输出目录" 更改为 "始终复制"
10. 在该文件的顶部, 将以下内容添加到现有的     "using" 语句列表中:
              复制
              using Microsoft.Tools.Deploy;
              using System.Net;
11. 在     "static void Main (...)" 内部, 添加以下代码:
              复制
              RemoteDeployClient client =     RemoteDeployClient.CreateRemoteDeployClient();
              client.Connect(new ConnectionOptions()
              {
                Credentials = new     NetworkCredential("DevToolsUser", string.Empty),
                IPAddress =     IPAddress.Parse(args[0])
              });
                  client.RemoteDevice.DeleteFile(@"C:\Data\Users\DefaultAccount\AppData\Local\DevelopmentFiles\VSRemoteTools\x86\CoreCLR\mscorlib.ni.dll");
12. 生成-> 生成解决方案
13. 在包含已编译程序的文件夹 (例如     C:\MyProjects\HoloLensDeploymentFix\bin\Debug) 上打开命令提示符
14. 运行可执行文件, 并提供设备的 IP 地址作为命令行参数。 (如果通过 USB 连接, 则可以使用     127.0.0.1, 否则使用设备的 WiFi IP 地址。)例如,     "HoloLensDeploymentFix 127.0.0.1"
15. 退出该工具而不使用任何消息 (这只需要几秒钟) 后, 你将能够从 Visual     Studio 2017 或更高版本进行部署和调试。 不需要继续使用该工具。

 

来自 <https://docs.microsoft.com/zh-cn/windows/mixed-reality/hololens-known-issues> 