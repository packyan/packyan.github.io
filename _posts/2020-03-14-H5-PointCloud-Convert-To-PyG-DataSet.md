#将h5格式点云数据集转化为PyG DataSet

## 导入点云数据集

 

```python
def load_data(partition):

  download()

  BASE_DIR = os.path.dirname(os.path.abspath(__file__))

  DATA_DIR = os.path.join(BASE_DIR, 'data')

  all_data = []

  all_label = []

  for h5_name in glob.glob(os.path.join(DATA_DIR, 'modelnet40_ply_hdf5_2048', 'ply_data_%s*.h5'%partition)):

​    f = h5py.File(h5_name)

​    data = f['data'][:].astype('float32')

​    label = f['label'][:].astype('int64')

​    f.close()

​    all_data.append(data)

​    all_label.append(label)

  all_data = np.concatenate(all_data, axis=0)

  all_label = np.concatenate(all_label, axis=0)

  return all_data, all_label

 

 

从h5文件中导入

 

构造dataset

class ModelNet40(Dataset):

  num_classes = 40

  def __init__(self, num_points, partition='train'):

​    self.data, self.label = load_data(partition)

​    self.num_points = num_points

​    self.partition = partition    

 

  def __getitem__(self, item):

​    pointcloud = self.data[item][:self.num_points]

​    label = self.label[item]

​    if self.partition == 'train':

​      pointcloud = translate_pointcloud(pointcloud)

​      np.random.shuffle(pointcloud)

​    return pointcloud, label

 

  def __len__(self):

​    return self.data.shape[0]

 

 

预处理

def translate_pointcloud(pointcloud):

  xyz1 = np.random.uniform(low=2./3., high=3./2., size=[3])

  xyz2 = np.random.uniform(low=-0.2, high=0.2, size=[3])

​    

  translated_pointcloud = np.add(np.multiply(pointcloud, xyz1), xyz2).astype('float32')

  return translated_pointcloud

 

 

def jitter_pointcloud(pointcloud, sigma=0.01, clip=0.02):

  N, C = pointcloud.shape

  pointcloud += np.clip(sigma * np.random.randn(N, C), -1*clip, clip)

  return pointcloud

 

构造Pyg数据集

 

class ModelNet(InMemoryDataset):

  r"""The ModelNet10/40 datasets from the `"3D ShapeNets: A Deep

  Representation for Volumetric Shapes"

  <https://people.csail.mit.edu/khosla/papers/cvpr2015_wu.pdf>`_ paper,

  containing CAD models of 10 and 40 categories, respectively.

 

  .. note::

 

​    Data objects hold mesh faces instead of edge indices.

​    To convert the mesh to a graph, use the

​    :obj:`torch_geometric.transforms.FaceToEdge` as :obj:`pre_transform`.

​    To convert the mesh to a point cloud, use the

​    :obj:`torch_geometric.transforms.SamplePoints` as :obj:`transform` to

​    sample a fixed number of points on the mesh faces according to their

​    face area.

 

  Args:

​    root (string): Root directory where the dataset should be saved.

​    name (string, optional): The name of the dataset (:obj:`"10"` for

​      ModelNet10, :obj:`"40"` for ModelNet40). (default: :obj:`"10"`)

​    train (bool, optional): If :obj:`True`, loads the training dataset,

​      otherwise the test dataset. (default: :obj:`True`)

​    transform (callable, optional): A function/transform that takes in an

​      :obj:`torch_geometric.data.Data` object and returns a transformed

​      version. The data object will be transformed before every access.

​      (default: :obj:`None`)

​    pre_transform (callable, optional): A function/transform that takes in

​      an :obj:`torch_geometric.data.Data` object and returns a

​      transformed version. The data object will be transformed before

​      being saved to disk. (default: :obj:`None`)

​    pre_filter (callable, optional): A function that takes in an

​      :obj:`torch_geometric.data.Data` object and returns a boolean

​      value, indicating whether the data object should be included in the

​      final dataset. (default: :obj:`None`)

  """

 

  urls = {

​    '10':

​    'http://vision.princeton.edu/projects/2014/3DShapeNets/ModelNet10.zip',

​    '40': 'http://modelnet.cs.princeton.edu/ModelNet40.zip'

  }

  

  def __init__(self, root, name='40', train=True, transform=None,pre_transform=None,

​         SamplePoints=1024, pre_filter=None):

​    assert name in ['10', '40']

​    self.name = name

​    self.SamplePoints = SamplePoints

​    super(ModelNet, self).__init__(root, transform, pre_transform,

​                    pre_filter)

​    path = self.processed_paths[0] if train else self.processed_paths[1]

​    

​    self.data, self.slices = torch.load(path)

​    print(path)

 

  @property

  def raw_file_names(self):

​    return [

​      

​    ]

 

  @property

  def processed_file_names(self):

​    return ['training.pt', 'test.pt']

 

  def download(self):

​    pass

 

  def process(self):

​    torch.save(self.process_set('train'), self.processed_paths[0])

​    torch.save(self.process_set('test'), self.processed_paths[1])

 

  def process_set(self, dataset):

  

​     if(self.name == '40'):

​      dataloader = ModelNet40(partition= dataset, num_points=self.SamplePoints)

​    data_list = []

​    

​    for data, label in dataloader:

​      data = Data(pos = torch.tensor(data), y = torch.tensor(label))

​      data_list.append(data)

 

​    if self.pre_filter is not None:

​      data_list = [d for d in data_list if self.pre_filter(d)]

 

​    print(len(data_list), data_list[0].pos.shape)

​    return self.collate(data_list)

 

  def __repr__(self):

​    return '{}{}({})'.format(self.__class__.__name__, self.name, len(self))
```



