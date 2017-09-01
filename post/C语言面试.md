---
title: C语言面试
date: 2017-09-01 15:02:33
tags: [C面试]
categories: C语言
---
## 引言
曾经面试做过的一些面试题目

## C/C++题目：
### 1、
比较字符串
输出它们第一个不同字母的位置，大小写不敏感  // if(str1[i] == (str2[i] | 0x20))
int cmpstr(const char* str1,const char* str2)
{
    int i = 0;

    while(str1[i] != '\0' && str2[i] != '\0')
    {
        if(str1[i] == (str2[i] | 0x20))
	{
	    i++;
	    continue;
	}
	return i+1;
    }
    return 0;
}
### 2、
判断一个数是不是回文数，数字 1234321。
int judge(int num)
{
    int temp1 = 0,temp2 = 0,temp3 = num;

    while(num)
    {
        temp1 = num % 10;
	temp2 = temp2 * 10 + temp1;
	num = num / 10;
    }
    if(temp2 == temp3)
    {
        return 1;
    }
    return 0;
}
### 3、
比较两字符串长短，并返回结果。
int cmpstr(const char* str1,const char* str2)
{
    int i = 0,j = 0;

    while(str1[i])
    {
        i++;
    }

    while(str2[j])
    {
        j++;
    }

    return i - j;
} 
### 4、给一个字符串，编程取其中一个特定的字符并输出，返回值 int / char* 字符的偏移量 或 地址
int findch(const char* str,char ch)
{
    int i = 0;

    while(str[i])
    {
        if(ch == str[i])
	{
	    return i+1;
	}
	i++;
    }
    return 0;
} 
### 5、
是比较两个英文字符串的不相同的字符的位置（忽略字母大小写）
int cmpstr(const char* str1,const char* str2,int site[])
{
    int i = 0,j = 0;

    while(str1[i] != '\0' && str2[i] != '\0')
    {
        if(str1[i] == (str2[i] | 0x20))
	{
	    i++;
	    continue;
	}
	site[j] = i + 1;
	j++;
	i++;
    }

    return j;
}
### 6、
主函数调用一函数
如：检索出字符串中出现次数最多的那个字符，不考虑大小写，然后返回该字符。
char findch(const char* str)
{
    int i = 0,j = 0,ct1 = 0,ct2 = 0;
    char ch1 = '\0',ch2 = '\0';

    while(str[i])
    {
        j = 0;
	ct1 = 0;		//这两个条件不可忽略
        while(str[j++])
	{
	    if(str[i] == str[j])
	    {
	        ct1++;
		ch1 = str[i];
	    }
	}
	if(ct2 < ct1)
	{
	    ct2 = ct1;
	    ch2 = ch1;
	}
	i++;
    }
    return ch2;
}
### 7、
查找字符串中出现次数最多的字符
并返回该字符，只考虑小写字母，不考虑不同字母出现次数一样多的情况
char findch(const char* str)
{
    int i = 0,j = 0,ct1 = 0,ct2 = 0;
    char ch1 = '\0',ch2 = '\0';

    while(str[i])
    {
        if(str[i] < 'a' || str[i] > 'z')
	{
	    i++;
	    continue;
	}

        j = 0;
	ct1 = 0;		//这两个条件不可忽略

        while(str[j++])
	{
	    if(str[i] == str[j])
	    {
	        ct1++;
		ch1 = str[i];
	    }
	}
	if(ct2 < ct1)
	{
	    ct2 = ct1;
	    ch2 = ch1;
	}
	i++;
    }
    return ch2;
}
### 8、
输入一个整数n，计算不大于n的数中和7相关的数的个数，
包括能被7整出的数和含有字符7的数，例如：输入20，输出3（7、14、17）。
int ctseven(int num)
{
    int ct = 0,i = 0,temp1 = 0,temp2 = 0;

    if(num < 7)
    {
        return 0;
    }

    for(i = 7;i < num;i++)
    {
        if(i % 7 == 0)
	{
	    ct++;
	    continue;
	}

	temp2 = i;

        while(temp2)
	{
	    temp1 = temp2 % 10;
	    if(7 == temp1)
	    {
	        ct++;
	        break;
	    }
	    temp2 = temp2 / 10;
	}
    }
    return ct;
}
### 9、
输入一个整数将每一位上的奇数放在一个新整数中，高位放在高位，地位在低位。
int getnum(int num)
{
    int i = 1,newnum = 0,temp = 0;    

    while(num)
    {
        temp = num % 10;
	if(temp % 2 != 0)
	{
	    newnum = newnum + temp * i;
	    i *= 10;
	}
	num = num / 10;
    }
    return newnum;
}
### 10、
输入一串数，
将其最小的放在第一位，次小的放在最后一位，
再小的放在第二位，再再小的放在倒数第二位，以此类推。
void get_newnum(int num[],int n)
{
    int i = 0,j = 0,k = 0,t = 0,temp = 0;	// t k 两个变量均用于控制数组下标

    for(i = 0;i < n-1-k;i++)
    {
        for(j = i;j < n-1-k;j++)
	{
	    if(num[i] > num[j+1])
	    {
	        temp = num[i];
		num[i] = num[j+1];
		num[j+1] = temp;
	    }
	}
	t++;			//算法实现 k控制循环范围 t控制循环数组元素交换	
	if(t % 2 == 0)
	{
	    temp = num[i];
	    num[i] = num[n-i];
	    num[n-i] = temp;
	    i--;
	    k++;
	}
    }
}
### 11、
写一个函数，传入参数为应付钱数。
返回值为买家最少付出的钱的张数int get MoneyNum(int  iInputMoney)例如：买家应付351元，
最少张数为5.备注：可支付的钱币只有100、50、10、5、1不考虑2、20以及小数部分。
int getct_money(int money)
{
    int ct = 0;

    ct += money / 100;
    money %= 100;
    ct += money / 50;
    money %= 50;
    ct += money / 20;
    money %= 20;
    ct += money / 10;
    money %= 10;
    ct += money / 5;
    money %= 5;
    ct += money / 1;
    money %= 1;
   
    return ct;
}
### 12、
设有几个人围坐在一圈并按顺时针方向从1到几编号，从第S个人开始进行1到m的报数。报数到第M个人，此人出圈。再从他的下一个人重新开始1到M的报数，如此进行下一直到所有人都出圈为止，输出报数顺序。
void getrank(int num[],int n,int num1,int num2,int newnum[])
{
    struct rank
    {
        int number;
	struct rank* next;
    };
    int i = 0,j = 0;
    struct rank* p = NULL;
    struct rank* temp1 = NULL;
    struct rank* temp2 = NULL;
    struct rank* phead = NULL;

    phead = p = calloc(1,sizeof(struct rank));
    p->number = num[0];

    for(i = 1;i < n;i++)
    {
        p->next = calloc(1,sizeof(struct rank));
        p->next->number = num[i];
	p = p->next;
    }

    p->next = phead;			//形成一个环形链表

    while(phead->number != num1)	//找出从谁开始报数
    {
	phead = phead->next;
    }
    
    for(j = 0;j < n;j++)			
    {
        for(i = 0;i < num2-1;i++)	//报数到num2时跳出循环 phead指向的number就是要出圈的数字
    	{
	    temp1 = phead;
	    phead = phead->next;
    	}
	newnum[j] = phead->number;
	temp2 = phead;
	phead = phead->next;
	free(temp2);			//出圈后删除该节点
	temp1->next = phead;	 
    }
}
### 13、
对姓氏进行排名
Char str[ ]=”zhang   wang   li    zhao”
Char str_ new[ ]=”li  wang   zhang   zhao”
void getstr(char str1[],int n,char str2[])
{
    int i = 0,j = 0,k = 0;	//i j k 均用于数组下标
    int len = strlen(str1);
    char* str[n];
    char* temp = NULL;

    for(i = 0;i < n;i++)
    {
        str[i] = calloc(1,len);
    }

    i = 0;		//while 一定要初始化
    while(str1[i])	//str[i++] 不可这样写
    {
        if(str1[i] == ' ')
	{
	    k = 0;
	    j++;
	    i++;	//while continue前 i++	
	    continue;
	}
	str[j][k] = str1[i];
	i++;
	k++;
    }

    for(i = 0;i < n-1;i++)
    {
        for(j = i;j < n-1;j++)
	{
	    if(strcmp(str[i],str[j+1]) > 0)
	    {
	        temp = str[i];
		str[i] = str[j+1];
		str[j+1] = temp;
	    }
	}
    }
   
    strcpy(str2,str[0]);

    for(i = 1;i < n;i++)
    {
        strcat(str2," ");
        strcat(str2,str[i]);
    }
    
    for(i = 0;i < n;i++)
    {
        free(str[i]);
    }

}
### 14、
将一组整数中为奇数的数提取出来，高低位顺序不变。如：8 3 7 9 5 2 1 4-----》3 7 5 1
int getodd(int num1[],int n,int num2[])
{
    int i = 0,j = 0;

    for(i = 0;i < n;i++)
    {
        if(num1[i] % 2 != 0)
	{
	    num2[j] = num1[i];
	    j++;
	}
    }
    return j;
}
### 15、
一组2n+1个元素的正整形数组，按升序排序，然后将小于中间数值的成员替换为中间的值。（貌似还有：“位置不变”，不过实在不理解其含义，看了例子就不用关心它的意思了），例如：1,2,3,4,5，输出为：3,3,3,4,5，原型：int fun(int arry[],int n,char*output){return 0;}
int fun(int num[],int n,char* output)
{
    int i = 0,j = 0,temp = 0;
    
    for(i = 0;i < n-1;i++)
    {
        for(j = i;j < n-1;j++)
	{
	    if(num[i] > num[j+1])
	    {
	        temp = num[i];
		num[i] = num[j+1];
		num[j+1] = temp;
	    }
	}
    }
    
    j = n / 2;

    for(i = 0;i < j;i++)
    {
        num[i] = num[j];
    }

    for(i = 0;i < n;i++)
    {
        output[i] = num[i] + '0';
    }

    return num[j];
}
### 16、
输入一个四位的十进制整数，编程实现将这四位整数转化为十六进制的字符串，并输出十六进制的字符串（注意负数的处理）
int hex_to_str(int num,char str[])
{
    int i = 0,len = 0,temp = 0;
    char ch = '\0';

    while(num)
    {
	temp = num & 0xf;

        if(temp < 10)
	{
	    str[i] = '0' + temp;
	}
	else
	{
	    str[i] = 'a' + temp - 10;
	} 

	num = num >> 4;

	if(num < 0)
	{
	    num = num & 0xfffffff;
	}
	i++;
    }

    str[i] = '\0';
    len = strlen(str);

    for(i = 0;i < len / 2;i++)
    {
        ch = str[i];
	str[i] = str[len-1-i];
	str[len-1-i] = ch;
    }

    return len;
}
### 17、输入：一个四位的整数，比如：2367，输出：2+3+6+7=18
int int_to_str(int num,char str[])
{
    int i = 0,j = 0,k = 0,temp = num,sum = 0;
    
    while(temp)
    {
        temp /= 10;
	i++;
    }

    int* pnum = calloc(i+1,sizeof(int));
    for(j = i-1;j >= 0;j--)
    {
        pnum[j] = num % 10;
	sum += pnum[j];
	num = num / 10;
    }
    pnum[i] = sum;
   
    for(j = 0;j < i*2;j++)
    {
        if(j % 2 == 0)
	{
            str[j] = pnum[k] + '0';
	    k++;
	    continue;
	} 
	str[j] = '+';
    }

    j--;
    str[j] = '=';

    if(sum / 10)
    {
        str[j+1] = sum / 10 + '0';        
        str[j+2] = sum % 10 + '0';
	str[j+3] = '\0'
	free(pnum);
	return j+3;
    }

    str[j+1] = '0' + sum % 10;
    str[j+2] = '\0'
    free(pnum);
    return j+2;
}
### 18,将一个正整数转换为字符串
char* int_to_str(int num,char* str)
{
    int i = 0,j = 0,temp1 = 0,temp2 = num;

    while(num)
    {
        temp1 = num % 10;          
        num = num / 10;
	j++;
    }
    str[j] = '\0';
    for(i = j-1;i >= 0;i--)
    {
	temp1 = temp2 % 10;
	str[i] = '0' + temp1;
	temp2 = temp2 / 10;
    }
    return str;
}
### 19，
将一个字符串转换为正整数
int str_to_int(const char* str)
{
    int i = 0,sum = 0;

    while(str[i])
    {
        sum = sum * 10 + (str[i] - '0');
	i++;
    }
    return sum;
}
### 20,从长串中找到连续相同字符最长的字串 
int get_childstr(const char* str,char* childstr)
{
    int i = 0,ct1 = 0,ct2 = 0;
    char ch = '\0';

    while(str[i])
    {
        ct1 = '\0';			// 要比较 切莫忘记初始化比较量
        while(str[i] == str[i+1])
	{
	    ct1++;
	    i++;
	}
	if(ct2 < ct1)
	{
	    ct2 = ct1;
	    ch = str[i];
	}
	i++;
    }
    for(i = 0;i < ct2+1;i++)
    {
        childstr[i] = ch;
    }
    childstr[i] = '\0';

    return ct2+1;
}
### 21,
将一个字符串逆序
char* getstr(char str[])
{
    int i = 0;
    int len = strlen(str);
    char ch = '\0';

    for(i = 0;i < len / 2;i++)
    {
	ch = str[i];
	str[i] = str[len-i-1];
	str[len-i-1] = ch;
    }
    return str;
}
### 22,
搜索给定的字节
void* findbyte(const void* st,size_t len,char dat)
{
    int i = 0;
    char* str = (char*)st;
    
    while(str[i])
    {
	if(dat == str[i])
	{
	    return str + i;
	}
	i++;
    }
	
    return NULL;
}
### 23,将一个链表逆序
void oppsite_list(ST* head)
{
    ST* phead = NULL;
    ST* P = head;
    head = head->next;
    p->next = NULL;
    
    while(head)		//	循环结束后 p 变为头指针
    {
        phead = head->next
	head->next = p;
	p = head;		
	head = phead;
    }
}
### 24,
判断一个字节有多少个1
int getsize(char ch)
{
    int i = 8,temp = 0,ct = 0;
    while(i--)
    {
	temp = ch & 1;
	if(temp)
	{
	    ct++;
	}
	ch >>= 1;
    }
    return ct;
}
### 24,
直接插入排序 冒泡排序 选择排序
void rank(int num[],int n)	// 直接插入
{
    int i = 0,j = 0,temp = 0;

    for(i = 1;i < n;i++)
    {
        temp = num[i];
	for(j = i - 1;j >= 0 && temp < num[j];j--)	// temp<num[j]从小到大  temp>num[j]从大到小 
	{
	    num[j+1] = num[j];
	}
	num[j+1] = temp;
    }
}

void rank(int num[],int n)	// 冒泡
{
    int i = 0,j = 0,temp = 0;

    for(i = 0;i < n;i++)
    {
	for(j = n - 1;j >= i;j--)	 
	{
	    if(num[j+1] < num[j])
	    {
		temp = num[j+1];
		num[j+1] = num[j];
		num[i] = temp;
	    }
	}
    }
}

void myrank_min(int num[],int count)	// 冒泡
{
    int i = 0,j = 0,temp = 0;

    for(i = 0;i < count-1;i++)
    {
	for(j = i;j < count-1;j++)
	{
	    if(num[i] > num[j+1])
	    {
	        temp = num[i];
		num[i] = num[j+1];
		num[j+1] = temp;
	    }
	}
    }
}

void rank(int num[],int n)	// 选择
{
    int i = 0,j = 0,k = 0,temp = 0;

    for(i = 0;i < n;i++)
    {
	temp = num[i];
	k = i;
	for(j = i;j < n;j++)	 
	{
	    if(temp > num[j])
	    {
		temp = num[j];
		k = j;
	    }
	}
	num[k] = num[i];
	num[i] = temp;
    }
}
### 26,
判断一个字符串有多少个单词
int getsize(const char* str)
{
    int state = 0,i = 0,ct = 0;

    while(str[i])
    {
	if(str[i] == ' ')
	{
	    state = 1;
	    i++;
	    continue;
	}
	if(state)
	{
	    state = 0;
	    ct++;
	}
    }
    return ct+1;
}
###26,
实现strcmp
int strcmp(onst char* s1,const char* s2)
{
    int i = 0;

    while(s1[i] != '\0' && s2[i] != '\0')
    {
        if(s1[i] == s2[i])
	{
	    i++;
	    continue;
	}
	return s1[i] - s2[i];
    }
    if(strlen(s1) == strlen(s2))
    {
	return 0;
    }
    return strlen(s1) - strlen(s2);
}

int strcmp(onst char* s1,const char* s2)
{
    int i = 0;

    while(s1[i] != '\0' && s2[i] != '\0')
    {
        if(s1[i] == s2[i])
	{
	    i++;
	    continue;
	}
	return s1[i] - s2[i];
    }
    if(s1[i] == '\0')
    {
	return -1;
    }
    if(s2[i] == '\0')
    {
        return 1;
    }
    return 0;
}

## 总结
面试还是看基础






