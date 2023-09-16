#### prim

```c++
//存储邻接矩阵
//初始化shortedge
//[0]为adjvex，出发点start，默认为0;
//[1]为start到各点的距离，自身为0
   
//lowcost==0，代表顶点已经加入到集合U中
vector<int> qa;      //小容器，存放adjvex
vector<int> pl;      //小容器，存储lowcost
for(int i=0;i<n;i++){       //初始化，都是从V0进行计算
    qa.push_back(0);     //出发点都是V0
    pl.push_back(vt[0][i]);
}
    shortedge.push_back(qa);
    shortedge.push_back(pl);
    shortedge[1][0]=0;     //lowcost为0，即代表顶点加入到U中


//不断从U-V中找到min的shortedge-lowcost
	for(int i=0;i<n-1;i++){     //遍历U-V中剩余的结点
        //找到shortedge的最小值
        int min_edge=INT_MAX;
        int min_index;  //记录最小边的点的下标，
        for(int j=0;j<n;j++){
            if(shortedge[1][j] < min_edge && shortedge[1][j] != 0){     //要求不在集合U中
                min_edge=shortedge[1][j];
                min_index=j;
            }
        }  
        sum+=min_edge;  //保存最短边的值，可替换输出树的路径

        //开始维护更新后的shortedge
        shortedge[1][min_index]=0;      //代表加入集合U
        //开始找lowcost，把新加入U集合的邻接矩阵行，和shortedge的lowcost进行比较。
        //选择min，即可保证shortedge中保存的是集合U中到达其余结点的最短边
        for(int y=0;y<n;y++){
            if(vt[min_index][y] < shortedge[1][y]){
                shortedge[1][y] = vt[min_index][y];
                shortedge[0][y] = min_index;		//把adjvex改为自身下标，表示从自身出发
            }
        }
        
    }
```

