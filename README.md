#include<iostream>
#include<cstring>
#include<cmath>
using namespace std;
template<class T>
struct BiNode {

	T data;
	BiNode<T>* lch;
	BiNode<T>* rch;
};
template<class T>
class SNode
{
public:
	BiNode<T>* R;
	int tag;
};
template<class T>
class BiTree
{
private:
	void Create(BiNode<T>* &R, T data[], int i, int n);
	void Release(BiNode<T>* R);
	
public:
	BiNode<T>* root;
	BiTree(T data[], int n);
	void PreOrder(BiNode<T>* R);
	void InOrder(BiNode<T>* R);
	void PostOrder(BiNode<T>* R);
	void LevelOrder(BiNode<T>* R);
	~BiTree();
	int Count(BiNode<T>* R);
	int depth(BiNode<T>* R)
	{
		if (R == NULL)
			return 0;
		int left = depth(R->lch);
		int right = depth(R->rch);
		return 1 + ((left > right) ? left : right);
	}
	bool isbalance(BiNode<T>* R);
};
template<class T>
void BiTree<T>::Create(BiNode<T>*& R, T data[], int i, int n)
{
	if (i <= n)
	{
		if (data[i-1] == 0 || data[i-1] == '0')
			R = NULL;
		else {
			R = new BiNode<T>;
			R->data = data[i - 1];
			Create(R->lch, data, 2 * i, n);
			Create(R->rch, data, 2 * i + 1, n);
		}
	}
	else
		R = NULL;
}
template<class T>
 BiTree<T>::BiTree(T data[], int n)
{
	Create(root, data, 1, n);
}
template<class T>
void BiTree<T>::PreOrder(BiNode<T>* R)
{
/*	if (R != NULL)
	{
		cout << R->data;
		PreOrder(R->lch);
		PreOrder(R->rch);
	}
	*/
	BiNode<T>* st[100];
	int top = -1;
	while (top != -1 || R != NULL)
	{
		if (R != NULL)
		{
			cout << R->data;
			st[++top] = R;
			R = R->lch;
		}
		else
		{
			R = st[top--];
			R = R->rch;
		}
	}
}
template<class T>
void BiTree<T>::InOrder(BiNode<T>* R)
{
/*	if (R != NULL)
	{
		InOrder(R->lch);
		cout << R->data;
		InOrder(R->rch);
	} */
	BiNode<T>* st[100];
	int top = -1;
	while (top != -1 || R != NULL)
	{
		if (R != NULL)
		{
			st[++top] = R;
			R = R->lch;
			
		}
		else
		{
			R = st[top--];
			cout << R->data;
			R = R->rch;
		}
	}
}
template<class T>
void BiTree<T>::PostOrder(BiNode<T>* R)
{
/*	if (R != NULL)
	{
		PostOrder(R->lch);
		PostOrder(R->rch);
		cout << R->data;
	}   */
	SNode<T> s[100];
	int top = -1;
	do
	{
		while (R != NULL)
		{
			s[++top].R = R;
			s[top].tag = 1;
			R = R->lch;
		}
		while (top != -1 && s[top].tag == 2)
		{
			cout << s[top].R->data;
			top--;
		}
		if (top != -1 && s[top].tag == 1)
		{
			R = s[top].R->rch;
			s[top].tag = 2;
		}
	} while (top != -1);
}
template<class T>
void BiTree<T>::LevelOrder(BiNode<T>* R)
{
	BiNode<T>* queue[1000];
	int f = 0, r = 0;
	if (R != NULL)
		queue[++r] = R;
	while (f != r)
	{
		BiNode<T>* p = queue[++f];
		cout << p->data;
		if (p->lch != NULL)
			queue[++r] = p->lch;
		if (p->rch != NULL)
			queue[++r] = p->rch;
	}
}
template<class T>
void BiTree<T>::Release(BiNode<T>* R)
{
	if (R != NULL)
	{
		Release(R->lch);
		Release(R->rch);
		delete R;
	}
}
template<class T>
BiTree<T>::~BiTree()
{
	Release(root);
}
template<class T>
int BiTree<T>::Count(BiNode<T>* R)
{
	if (R == NULL)
		return 0;
	else
	{
		int m = Count(R->lch);
		int n = Count(R->rch);
		return m + n + 1;
	}
}
template<class T>
bool BiTree<T>::isbalance(BiNode<T>* R)
{
	if (R == NULL)
		return true;
	int left = depth(R->lch);
	int right = depth(R->rch);
	int diff = left - right;
	if (abs(diff) > 1)
		return false;
	else
		return isbalance(R->lch) && isbalance(R->rch);
}
char a[5005];
int main()
{
	cin.getline(a, 5000, '\n');
	int n = strlen(a);
	BiTree<char> bt(a, n);
	if (bt.isbalance(bt.root))
		cout << "True" << endl;
	else
		cout << "False" << endl;
	return 0;
}
