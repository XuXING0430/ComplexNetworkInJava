

# Complex Network In Java V1.0

This project is dedicated to complex network research using Java.

Researchers do not really want to focus on the process of object development, nor do they want to define many nodes and edges at the initial network. We just want to input an **adjacency matrix** or **adjacency list** that can represent the network connection and strength. Using these data, we can find many indicators, such as **degree**,, **average distance**, **network efficiency**, **maximum connected subgraph size**... 

That is to say, researchers only want to find the desired indicator in the simplest way. This project will help you.

## Property  V1.0:

For the time being, only undirected and unweighted networks are supported

* Adjacency matrix convert to Adjacency list
* Adjacency list convert to Adjacency matrix
* Degree
* Clustering coefficient
* Average path length
* Network efficiency
* Deliberately attack 

## Getting Started

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes. See deployment for notes on how to deploy the project on a live system.

### Prerequisites

* JDK(JRE)
* Network Data :Adjacency matrix
  or Adjacency list in a Excel(.xlsx) file

### Installing

* Just copy **complexNetwork_V1.0.jar** in your project folder in a IDE. And add this jar as a library.  
* Copy your Excel file in your project folder

## Running the tests

New a Java file to test. You can use a main method or Junit test.

### Example 1: If your data is a  Adjacency list. You need to convert it as a  Adjacency matrix first.

```java
/**
     * test_adi.xlsx
     * 
     * 2 1
     * 3 2
     * 4 2
     * 4 3
     * 5 1
     * 5 2
     * 5 3
     * @throws Exception
     */
    @Test
    public void testAdi() throws Exception {
        //adj
        String filepath = "src/main/resources/test_adi.xlsx";
        int[][] adjList = DataUtils.adjMatrixOrList(filepath,"list");

        int nodeNumber = DataUtils.findMax(adjList);

        int[][] adjMatrix = DataUtils.adjList2adjMatrix(adjList,nodeNumber);
        // print matrix
        DataUtils.printArray(adjMatrix);
    }
```

#### output：

If you use my **test_adi.xlsx**, this output is correct. Then you can take the next step. Please refer to   Example 2 after 34 lines.

```
0 1 0 0 1 
1 0 1 1 1 
0 1 0 1 1 
0 1 1 0 0 
1 1 1 0 0
```



###  Example 2: If your data is a  Adjacency matrix.

```java
   /**
     * test_matix.xlsx
     * 0	1	0	0	1
     * 1	0	1	1	1
     * 0	1	0	1	1
     * 0	1	1	0	0
     * 1	1	1	0	0
     *
     * node mumber：5； edge number：7
     * @throws Exception
     */
    public static void main(String[] args) throws Exception {
        //adj matrix path.You need to change this path
        String filepath = "src/main/resources/test_matix.xlsx";
        
        // "matrix" means that your data is Adjacency matrix
        int[][] matrix  = DataUtils.adjMatrixOrList(filepath,"matrix");

        int nodeNumber = matrix.length;
        //edge number
        int edgeNumber = GraphUtils.getEdgeNumber(matrix,nodeNumber);
        System.out.println("edge number:" + edgeNumber);

        // adj_matrix convert to adj_list
        int[][] adjList = DataUtils.adjMatrix2adjList(matrix,nodeNumber);
        System.out.println("adjacency list:");
        DataUtils.printArray(adjList);
        System.out.println("===============");
        
        // this is test adj_list convert to adj_matrix
//        int[][] adjMatrix = DataUtils.adjList2adjMatrix(adjList,nodeNumber);
//        DataUtils.printArray(adjMatrix);

        // degree
        int[] degree = GraphUtils.degree(matrix,nodeNumber);
        // aver_degree
        float averDegree = DataUtils.getAverageData(degree,nodeNumber);
        System.out.println("aver_degree:"+averDegree);
        System.out.println("degree" + Arrays.toString(degree));

        //cluster
        float[] cluster = GraphUtils.clustering(matrix,degree,nodeNumber);
        //aver_cluster
        float averCluster = DataUtils.getAverageData(cluster,nodeNumber);
        System.out.printf("aver_cluster：%f \n",averCluster);
        System.out.println("cluster:" + Arrays.toString(cluster));

        //floyd
        Floyd floyd = new Floyd(DataUtils.getFloydMatrix(matrix,nodeNumber));
        //distance matrix
        int[][] distanceMatrix = floyd.floyd();

        //aver_distance
        Float averDistance = DataUtils.getAverageData(distanceMatrix,nodeNumber);
        System.out.printf("aver_distance: %f \n",averDistance);
        //Net Efficiency
        float efficiency = GraphUtils.netEfficiency(distanceMatrix, nodeNumber);
        System.out.println("Net Efficiency: " + efficiency);

        //Sort the indicators to facilitate the next robustness test under deliberate attacks
        List<Integer> sortList = DataUtils.pairSort(degree);
        Integer[] sortIndex = sortList.toArray(new Integer[sortList.size()]);
        System.out.println("sort Index:"+ Arrays.toString(sortIndex));

	    //degree deliberate attacks
        int numberDelete = matrix.length;
        float[] netEff = RobustnessUtils.nodeDeliberatelyAttacks(matrix,sortIndex,numberDelete,nodeNumber);
        System.out.println("Net Efficiency Index changes: " + Arrays.toString(netEff));
    }

```

#### output：

If you use my **test_matix.xlsx**, this output is correct.

```
node number:5
edge number:7
adjacency list:
2 1 
3 2 
4 2 
4 3 
5 1 
5 2 
5 3 
===============
aver_degree:2.8
degree[2, 4, 3, 2, 3]
aver_cluster：1.533333 
cluster:[2.0, 1.0, 1.3333334, 2.0, 1.3333334]
aver_distance: 1.040000 
Net Efficiency: 0.85
Degree sort Index:[1, 4, 2, 3, 0]
Net Efficiency Index changes: [0.43333334, 0.1, 0.0, 0.0, 0.0]
```

## Built With

- [Maven](https://maven.apache.org/) - Dependency Management

## To be improved

- [ ] Betweenness
- [ ] Size of the largest subgraph
- [ ] Random attack
- [ ] Support undirected and weighted network
- [ ] Support directed and weighted network

## Contributing

Pull requests and issues are very welcome! Or you can contact me Email:xskjx0430@sina.com.

## Authors

- **XuXING0430** - *Initial work* 

Looking forward to your joining.

## Contact me

If you have any questions or suggestions you can contact me Email： xskjx0430@sina.com.

## License

Not yet.

