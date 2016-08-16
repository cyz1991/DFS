# DFS
package test;

import java.io.DataInputStream;
import java.io.IOException;

class G_shpath{
	final int MAX_V = 100;
	final int VISITED = 1;
	final int NOTVISITED = 0;
	final int Infinite = 100;
	//A[1..n][1..n]为图的相邻矩阵
	//D[i]i=1..n用来存储某起始顶点到i结点的最短路径
	//S[1.。n]用来记录顶点是否已经访问过
	//P[1.。n]用来记录最近经过的中间结点
	int[][] A = new int[MAX_V+1][MAX_V+1];
	int[] D = new int[MAX_V+1];
	int[] S = new int[MAX_V+1];
	int[] P = new int[MAX_V+1];
	int source,sink,N;
	int step;
	int top;        //栈指针
	int[] Stack = new int[MAX_V+1];   //栈空间
	DataInputStream in = new DataInputStream(System.in);
	DataInputStream infile;
	public G_shpath(){
		step = 1;
		top = -1;
	}
	public void init(){
		int i,j=0,weight = 0;
		boolean done = false;
		/*try{
			infile = new DataInputStream(new FileInputStream("D:/singlelist.txt"));
		}
		catch(IOException e){
			System.err.println("file not found!");
			System.exit(1);;
		}
		//读取图的结点数
		try{
			N = infile.readInt();
		}
		catch(IOException e){}*/
		N = 7;
		System.err.println(N+"a");
		for(i=1;i<=N;i++) 
			for(j=1;j<=N;j++)
				A[i][j] = Infinite;
		System.err.println(N+"b");
		A[1][2]=4;
		A[1][3]=6;
		A[1][4]=6;
		A[2][3]=1;
		A[2][5]=7;
		A[3][5]=6;
		A[3][6]=4;
		A[4][3]=2;
		A[4][6]=5;
		A[5][7]=6;
		A[6][5]=1;
		A[6][7]=8;
		/*while(done==false){ }
		try{
			infile.close();
		}catch(IOException E){}*/
		System.err.println(N+"c");
		try{
			System.out.println("Enter source node: ");
			source = Integer.valueOf(in.readLine()).intValue();
			System.out.println("Enter sink node: ");
			sink = Integer.valueOf(in.readLine()).intValue();
		}
		catch(IOException e){}
		//起始各数组的初值
		for(i=1;i<=N;i++){
			S[i] = NOTVISITED;       //各顶点设置为未访问过
			D[i] = A[source][i];     //记录起始顶点到个顶点最短距离
			P[i] = source;
		}
		S[source] = VISITED;       //起始顶点已经访问过
		D[source] = 0;
	}
	public void access(){
		int I,t;
		for(step=2;step<=N;step++){
			//minD返回值t使得D[t]为最小
			t = minD();
			S[t] = VISITED;
			//找出经过t点会使路径缩短的结点
			for(I=1;I<=N;I++)
				if((S[I]==NOTVISITED)&&(D[t]+A[t][I]<=D[I])){
					D[I] = D[t]+A[t][I];
					P[I] = t;
				}
			output_step();
		}
	}
	public int minD(){
		int i,t = 0;
		int minimum = Infinite;
		for(i=1;i<=N;i++){
			if((S[i]==NOTVISITED)&&(D[i]<minimum)){
				minimum = D[i];
				t = i;
			}
		}
		return t;
	}
	//显示目前的D数组与P数组情况
	public void output_step(){
		int i;
		System.out.println("Step#"+step);
		System.out.println("*******************");
		for(i=1;i<=N;i++)
			System.out.print("  D["+i+"]");
		System.out.println(" ");
		for(i=1;i<=N;i++)
			if(D[i]==Infinite)
				System.out.println("-------");
			else
				System.out.print("    "+D[i]);
		System.out.println(" ");
		System.out.println("********************");
		for(i=1;i<=N;i++)
			System.out.print("  P["+i+"]");
		System.out.println(" ");
		for(i=1;i<=N;i++)
			System.out.print("    "+P[i]);
		System.out.println(" ");
	}
	//显示最短路径
	public void output_path(){
		int node = sink;
		//判断是否起始顶点等于终点或者没有路径到终点
		if((sink==source)||(D[sink]==Infinite)){
			System.out.println("Node "+source+" has no path to Node "+sink);
			return;
		}
		System.out.println(" ");
		System.out.println("The shortest path from V"+source+" to V"+sink+":");
		System.out.println("------------------------------------------------");
		//由终点开始讲上一次经过的中间结点入栈直到起始结点
		System.out.println(" V"+source);
		while(node!=source){
			Push(node);
			node = P[node];
		}
		while(node!=sink){
			node = Pop();
			System.out.print("--"+A[P[node]][node]+"-->");
			System.out.print("V"+node);
		}
		System.out.println(" ");
		System.out.println("Total length:"+D[sink]);
	}
	public void Push(int value){
		if(top>=MAX_V){
			System.out.println("Stack overflow");
			System.exit(1);
		}
		else
			Stack[++top] = value;
	}
	public int Pop(){
		if(top<0){
			System.out.println("Stack empty!");
			System.exit(1);
		}
		return Stack[top--];
	}
	public static void main(String args[]){
		G_shpath obj = new G_shpath();
		int t,I;
		obj.init();
		obj.output_step();
		obj.access();
		obj.output_path();
	}
}
