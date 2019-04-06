//Logistic regression using pure c++ coding
#include<bits/stdc++.h>65432
using namespace std;
double input[768][10];
double coefficient[10];
double l_rate=0.1;
int in_no;
void printall()
{
	for(int i=0;i<700;i++)
	{
		for(int j=0;j<10;j++)
		{
			cout<<input[i][j]<<"\t";
		}
		cout<<endl;
	}
}
void print_coeff()
{
	for(int i=0;i<8;i++)
	{
		cout<<coefficient[i]<<"\t";
	}
	cout<<endl;
}
//generate random number
double getRand(void)
{
 return ((double)rand())/(double)RAND_MAX;
}
void random_coefficient()
{
	for(int i=0;i<10;i++)
	coefficient[i]=(double)rand() / (double)((unsigned)RAND_MAX+1);
}
float findmax(int pos)
{
	float a=INT_MIN;
	for(int i=0;i<768;i++)
	{
		if(a<input[i][pos])
			a=input[i][pos];
	}
	return a;
}
float findmin(int pos)
{
	float a=INT_MAX;
	for(int i=0;i<768;i++)
	{
		if(a>input[i][pos])
			a=input[i][pos];
	}
	return a;
}
void normalize(int pos)
{
	float max=findmax(pos);
	float min=findmin(pos);
	float val=max-min;
	for(int i=0;i<768;i++)
	{
		input[i][pos]=(input[i][pos]-min)/val;
	}
}
float sigmoid(float x)
{
	return (1/(1+exp(-1*x)));
}
double yhat,expected;
void predict(int cols)
{
	yhat=0,expected=0;
	yhat=coefficient[0];
	for(int i=0;i<8;i++)
		yhat=yhat+coefficient[i+1]*input[cols][i];
	expected=sigmoid(yhat);
	//cout<<"expected "<<expected<<"output"<<input[cols][9]<<endl;
}

double error;
void train()
{

	for(int ephoch=0;ephoch<30000;ephoch++)
	{
			for(int i=0;i<500;i++)
	{
			
			error=0.0;	
			predict(i);
			error=expected-input[i][9];
			//cout<<error<<endl;
			coefficient[0]=coefficient[0]-l_rate*error*expected*(1.0-expected);
			for(int k=0;k<8;k++)
			{
				coefficient[k+1]=coefficient[k+1]-l_rate*expected*error*(1.0-expected)*input[i][k];
			}			
	}
		
	}

}
void test()
{
	int count=1;
	for(int i=501;i<768;i++)
	{
		predict(i);
		
		if((expected<=0.4 && input[i][9]==0) || (expected>0.6 && input[i][9]==1) )
		{
			cout<<"expected  "<<expected<<"  output  "<<input[i][9]<<endl;
			count++;
		}
	}
	cout<<count;
}
int main()
{
	srand(time(0));
	 std::fstream myfile1("D:\\strp.txt", std::ios_base::in);

    double a;
    int count=0;

    int bount=0;
	int flag=0;

    while (flag<768)
    {
            bount=0;
            int l=0;
            while(myfile1 >> a)
             {
               
                 input[flag][l]=a;
                 l++;
                 bount++;
                 //cout<<input[flag][l];
                 if(bount==10)
                   {
						break;
					}
             }
             flag++;
            // cout<<endl;
	}

	
	for(int i=0;i<8;i++)
	{
		normalize(i);
	}
	random_coefficient();
	print_coeff();
	train();
	print_coeff();
	test();
	
//	printall();	
		
}
