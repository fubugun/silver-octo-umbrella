# 字符串
---
## 最长相同字符串的位置
---
### 编写程序，求字符串中的字符平台。
### 说明：一个字符串中的任意一个子序列，若子序列中各字符均相同则成为字符平台。编程要求：输入任意一个长度不超过 100 的字符串 S，输出 S 中长度最大的所有字符平台的起始位置及所含字符，注意字符平台有可能不止一个。
### 例如：输入：abcddefggghijkLmmn
### 输出：8, g
---
遍历字符串，统计 **连续相同字符的长度**。

### 核心思想：

1. 如果当前字符和前一个字符相同  
   → 平台继续，长度 `currentsize++`

2. 如果字符不同  
   → **当前平台结束，需要判断**：

- 是否 **大于最大平台**
- 是否 **等于最大平台**

并记录平台的：

```
起始位置
平台字符
```

**关键技巧：**

```
for(i = 1; i <= n) 
```

这样 **最后一个平台会自动处理**，无需额外代码。

---


```
`results` 结构：
results[0][i]  → 平台起始位置
results[1][i]  → 平台字符
```
```c
memset
需要包含头文件：
#include <cstring>

基本语法
memset(地址, 填充值, 字节数);

含义：
从指定地址开始，把后面的若干字节全部设置成某个值

最常见用法：数组清零
int a[100];
memset(a, 0, sizeof(a));

数组的大小 sizeof(数组名字)
```


---





```c

#include<stdio.h>
#include<string.h>
//引入string.h 可以使用memset(results,0,sizeof(results)) 函数
#define MAX 105 //就可以定义数组

int main(){
	char s[MAX];
	scanf("%s",s);
	int n = strlen(s);
	int results[2][MAX];//两行MAX列
	results[0][0] = 0;//位置 
	results[1][0] = s[0];//字符的ASCII 

	int resultcount = 0;
	int currentsize = 1;
	int currentlocal = 0;//记录当前位置 最后存储进results 而不是存储i 

	int maxsize = 1;
	
	for(int i = 1; i <= n; i++){//加上等于号 就不需要下面再判断字符串末尾 
		if(s[i] == s[i - 1]){
			currentsize++;//符合 
		}else{//不符合   说明被打断了 需要处理这个字符前面的字符串
			if(currentsize > maxsize){
				resultcount = 0;
				memset(results, 0, sizeof(results));
				maxsize = currentsize;
				results[0][0] = currentlocal;//被打断了 那么长的应该是前一个  前一个的位置是currentlocal
				results[1][0] = s[currentlocal];//被打断了 那么长的应该是前一个 
			} 
			else if(currentsize == maxsize){//这里是else if 而不是if 因为if的话 就会顺序执行 比如maxsize是1 现在还是1 就会重复执行 
				resultcount++ ;
				results[0][resultcount] = currentlocal;
				results[1][resultcount] = s[currentlocal];
			}
		currentsize = 1;
		currentlocal = i;//被打断的时候currentlocal需要改变  改成现在处理的字符串的第一个位置
		}


	}

	//处理末尾的字符串 
	//现在因为上面的 i<= n 这个等于号  所以末尾的字符串不需要单独判断
			
		/*if(currentsize > maxsize){
				resultcount = 0;
				memset(results, 0, sizeof(results));
				maxsize = currentsize;
				results[0][0] = currentlocal;//存储的是第一次出现的位置 是cunrrentloacl 
				results[1][0] = s[n - 1];
			} 
			if(currentsize == maxsize){
				resultcount++ ;
				results[0][resultcount] = currentlocal;
				results[1][resultcount] = s[currentlocal];
			}
		*/

	
	//输出
	for(int i = 0; i <= resultcount; i++){
		printf("%d,%c\n",results[0][i] + 1,results[1][i]);
	} 
	
	return 0;
}
```
---



## 对输入的所有的字符串进行排序并输出，要求输入若干个字符串，字符串之间以空格分隔，以换行符结束输入，对字符串进行排序，然后从小到大输出。
#### 方法一   用string库
```c
#include <iostream>
#include <string>//string 的库函数 
#include <algorithm>//排序函数sort的库函数 

using namespace std;

//排序规则 按从小到大排 
bool cmp(string a, string b){//注意函数参数 
	return a < b;//告诉sort谁排在谁前面 
}

int main(){
	
	string s[100];//string是字符串类型  string[100]储存的是100个字符串 
	int i = 0;//标记s的个数 
	
	while(1){//读入字符串 以空格分隔     以换行符结束 
		
		if(cin.peek() == '\n') break;
		
		cin>>s[i++];//读入  s[i] 储存一个字符串 
	}
	
	//  sort(开始地址, 结束地址, 比较规则) 
	sort(s,s + i, cmp);//排序 规则按照cmp的规则 
	
	for(int j = 0; j < i; j++){//输出前i-1个字符串 以空格分隔 
		cout<<s[j]<<" ";//加上空格 <<“ ” 
	}
	
	cout<<s[i]<<endl;//单独输出最后一个字符串 防止最后一个字符串后面多一个空格 

}
```
---
#### string的用法
```c
	库 	<string>
	操作	s = "123"
		string c = a + b;
		s.empty();
```
---
#### 方法二 用vector
```c
#include <iostream>
#include <vector>//引入一系列库函数 
#include <string>
#include <algorithm>

using namespace std;

bool cmp(string a, string b){//注意函数参数 
	return a < b;//告诉sort谁排在谁前面 
}

int main(){

	vector<string>vec;	//vec存储string类型的数据 
	string str;
	
	while(1){
		
		if(cin.peek() == '\n') break;
	
		cin>>str;//先用str储存数据 
		vec.push_back(str);//再把数据压入vec   vec.pushback(str)

	}
	
	sort(vec.begin(), vec.end(), cmp);//sort函数的参数是开始位置 结束位置 而位置是vec.begin()  vec,end() 
	
	for(int j = 0; j < vec.size() - 1; j++){//不需要用i来计数 直接用vec.size()就行 
		cout<<vec[j]<<" ";//加上空格 <<“ ” 
	}
	
	cout<<vec[vec.size() - 1]<<endl;//单独输出最后一个字符串 防止最后一个字符串后面多一个空格 
	
}
```
---
#### vector的用法
```c
	库 	<vector>  <algorithm>
	定义 	vector<int>v;
	操作 	v.pushback(x);
		v.size()
		v[i]
	位置	v.begin()   v.end()
```
---
#### stack queue用法
```c
	库 	<stack> <queue>
	定义 	stack<int>stk
	操作 	stk.empty()
		stk.top()

		que.front()
		que.back()
		q.empty()
```
---


## 去掉括号
---
### 编写程序，输入一表达式（可能包含字母，括号，加号，减号）
### 在保持表达式值不变的情况下
### 将输入的表达式中的括号全去掉
### 并输出去掉括号后的表达式

	遇到） 才需要弹出栈
	遇到（ 压入栈时  需要看前一个字符
	要用else if 而不是if  用于减少逻辑判断

```c
#include <iostream>
#include <string>
#include <stack>
using namespace std;

int main(){

    string s;
    cin >> s;  // 读入表达式，例如 a-(b-c)

    stack<int> stk;  
    stk.push(0);  // 栈顶表示当前符号环境
                  // 0 表示不翻转符号
                  // 1 表示需要翻转符号

    for(int i = 0; i < s.length(); i++){

        // 如果是字母或数字，直接输出
        if(isalpha(s[i]) || isdigit(s[i])){
            cout << s[i];
        }

        // 遇到左括号
        else if(s[i] == '('){

            // 如果 '(' 在最开头
            if(i == 0)
                stk.push(stk.top());  // 继承上一层符号环境

            // 如果 '(' 前面是 +
            else if(s[i-1] == '+')
			//+：继承当前环境
                stk.push(stk.top());  // 符号环境不变

            // 如果 '(' 前面是 -
            else if(s[i-1] == '-')
			//-：翻转当前环境
                stk.push(!stk.top()); // 符号翻转

            // 其它情况（例如字母后面接括号）
			//或者括号后面接括号  都直接继承当前环境
            else
                stk.push(stk.top());  // 继续继承当前环境
        }

        // 遇到右括号，说明当前括号处理完
        else if(s[i] == ')'){
            stk.pop();  // 回到上一层符号环境
        }

        // 遇到 +
        else if(s[i] == '+'){

            // 如果当前环境不翻转
            if(stk.top() == 0)
                cout << "+";

            // 如果当前环境翻转
            else
                cout << "-";
        }

        // 遇到 -
        else if(s[i] == '-'){

            // 当前环境不翻转
            if(stk.top() == 0)
                cout << "-";

            // 当前环境翻转
            else
                cout << "+";
        }
    }

    cout << endl;
}
```
---
---
## 输入两个字符串，输出两个字符串的交集，对于重复的字符，只输出一次，并且按照升序输出交集中的字符。
---
自动去重容器  set
```c
#include <iostream>
#include <set>
#include <string>
using namespace std;

int main(){
	
	string a, b;
	cin >> a >> b;//读入两个字符串 中间以空格分隔 
	
	set<char> s;//set 能够自动去重 并且让元素按升序排序 

	//遍历a中元素  如果在b里找到了  就把他放进set容器里
	for(int i = 0; i < a.length(); i++){
			char c = a[i];
			if(b.find(c) != string::npos)  s.insert(c);//b.find( ) != string::nops 表示在b里 找到了这个元素 ；  s.insert(c) 
		}

		//输出set里的元素
		for(set<char>::iterator it = s.begin(); it != s.end(); it++)
		// set<char>::iterator	定义一个“指向set的指针”
		//it	这个指针的名字
		//s.begin()	让它指向set里的第一个元素
		//s.end() 不是最后一个元素          它是“最后一个元素的后面”
		//it++  把指针往后移动一个元素
		{
			cout << *it;//输出指针指向的元素 用 *it 
		}

}



/*
set<char>::iterator
意思是：  这个 set 里面定义的 iterator 类型
翻译成人话：   set 自己内部的 迭代器
*/



/*
string :: npos
意思是：  string 这个类里面的一个常量
npos  表示没找到 
*/
```
---
#### set使用方法
```c
库函数 		#include<set>
创建set		set<int> s;
插入元素		s.insert(5);
插入元素 	1. set自动按升序排序 2. 去重
遍历		for(int x : s)
		{
    			cout << x << " ";
		}

		s.begin()  	s.end()
查找元素		s.find(5);
删除元素		s.erase(5);
判断元素个数	s.size();
清空集合		s.clear();
字符串去重（！！）	
		string str = "aabbcc";

		set<char> s;

		for(char c : str)
		{
			s.insert(c);
		}

		for(char c : s)
		{
			cout << c;
		}	
```
---
---
## 中缀表达式转后缀表达式
__栈顶元素是（时 不比较__
1.初始化一个空栈 opStack 用来存操作符

2.从左到右扫描中缀表达式：

	a操作数 → 直接输出

	b左括号 '(' → 压栈

	c右括号 ')' → 弹出栈中操作符输出，直到遇到左括号 '('//这里老忘

	d操作符 (+ - * / ^) →

		弹出栈中优先级 ≥ 当前操作符的符号，输出
		并且栈顶元素是（时 不比较
		然后当前操作符压栈

3.扫描结束 → 弹出栈中剩余操作符输出

注意 所有有弹出操作的循环 while的条件里必须有 且先有    栈不为空

---
```c
#include <iostream>
#include <stack>
#include <string>
using namespace std;

//判断优先级 
int presedence(char c){
	if(c == '^') return 3;//次方是最高级 
	if(c == '*' || c == '/') return 2;
	if(c == '+' || c == '-') return 1;
	return 0;
}

//中缀转逆波兰 
void intopost(string s){
	stack <char> stk;
	int i = 0;
	
	while(i < s.length()){//s的结束符不是'\0'  不能用s[i++]!='\0' ; 不要写i++  因为参与比较的是i 但是比较完了 i就变成i+1了 导致下面的运算都把第一个字符跳过了 
		if((s[i] >= '0' && s[i] <= '9') || (s[i] >= 'a' && s[i] <= 'z') || (s[i] >= 'A' && s[i] <= 'Z')) cout <<s[i];//操作数 
		else if(s[i] == '(') stk.push(s[i]);
		else if(s[i] == ')'){
			//遇到） 一直弹出 直到遇到（ 然后把(也弹出 
			while(!stk.empty() && stk.top() != '('){//注意!stk.empty() 
				cout << stk.top();
				stk.pop();
			}
			stk.pop();//弹出 '('
		}else{
			while(!stk.empty() && presedence(stk.top()) >= presedence(s[i]) && stk.top != '('){//一直弹出优先级大于等于当前符号的运算符 
			//并且当栈顶是（ 时 不参与比较
				cout << stk.top();
				stk.pop();
			}
			stk.push(s[i]);//弹出其他的再把它压入 
		} 
		i++;
	}
	
	//处理栈内剩余符号 
	while(!stk.empty()){
		cout << stk.top();
		stk.pop();
	}
	
	cout<<endl;
} 

//主函数 
int main(){
	string s;
	cin >> s;
	intopost(s);
}
```
---
---
## 输入字符串 s，整数 n，输出长度为 n 的 s 的没有重复字符的顺序子串。
例：输入：abccdef 2
输出：ab,bc,cd,de,ef<回车>


这道题判断的是子串 子串是连在一起的几个字母
不是随机选几个字母拼接成一个字符串

```c
1.输出前几个后面是逗号 最后一个是回车  用——vector

2.substr()
	s.substr(开始位置, 长度)

	例如：
	s = "abcdef"
	s.substr(1,2) = "bc"
```
```c
#include <iostream>
#include <vector>
#include <string>
using namespace std;

//判断是否有重复字符  用两个for循环就行 
int norepeat(string s){
	for(int i = 0; i < s.length(); i++){
		for(int j = i + 1; j < s.length(); j++){
			if(s[i] == s[j]) return 0;
		}
	}
	return 1;
}

int main(){
	string s;
	int n;
	cin >> s >> n;
	vector<string> vec;
	
	for(int i = 0; i < s.length(); i++){
		string result = s.substr(i, n);//s.substr(开始位置，子串长度) 
		if(norepeat(result)) vec.push_back(result);//符合要求 送入vec中 
	}
	
	//前面的输出后有逗号 最后一个输出回车  用 vec输出 
	for(int i = 0; i < vec.size(); i++){
		cout << vec[i];
		if(i != vec.size() - 1) cout << ",";//输出逗号需要用双引号 
	}
	
	cout << endl;
	
}
```
---
---
## 输入多个字符串，中间用空格分开，以回车结束，将这些字符串链接在一起，并且连接后第二个字符串中不含有第一个字符串的字符，第三个字符串不含有第一个和第二个字符串的字符。
例：输入 ab bb cc cd
输出：abccd<回车>
```c
#include <iostream>
#include <string>
#include <vector>
using namespace std;

int main(){
	vector<string> vec;//读入的字符串 放在vec容器里 
	string result = "";//结果串 先置为空 “” 
	int used[300] = {0};//标记出现过的字符  用空间换时间 
	string s;
	
	while(1){
    	string s;
    	cin >> s;
    	vec.push_back(s);

		
    	if(cin.peek()== '\n') break;
}
	
	for(int i = 0; i < vec.size(); i++){//处理每个字符串 
		
		for(int j = 0; j < vec[i].length(); j++){//处理每个字符 
			if(used[vec[i][j]] == 0) result += vec[i][j];//这个字符没在used里出现 就加入结果  result 直接加ASCII就行 不需要单引号 
		}
		
		for(int j = 0; j < vec[i].length(); j++){//处理当前字符串   把这个字符串中的字符都标记为使用过 
			if(used[vec[i][j]] == 0) used[vec[i][j]] = 1;
		}	
	}
	
	cout << result <<endl;
	
}

```
```c
cin >> 之后接 getline
一定要先 cin.ignore();  丢弃输入缓冲区的一个字符
```
---

---
## 输入一个字符串(含空格),去掉重复字符的最后一个
例如:abbbcdedf
输出:abbcdef

__字符串长度为1 单独处理__

	要处理的某段只有一个字母 则单独处理
	substr(开始位置，长度)
```c
#include <iostream>
#include <string>
#include <vector>
using namespace std;

int  main(){
	string s;
	getline(cin, s);//读入带空白格的字符串 getline(cin,s)
	 
	int currentlength = 1;//currentlength 表示当前字符串的长度  或者说要提取的字符串的长度 
	int currentlocal = 0;//表示现在处理的这段字符串的开始位置
	 
	vector<string> result;//存储结果  将处理好的每段字符串存入vec 
	
	for(int i = 1; i <= s.length(); i++){
		if(s[i] == s[i-1]){
			currentlength++;//和前面的一样   ——往后遍历就行 
		}
		else{//碰到不一样的了  处理这个字符前的字符串 
			string temp = "";//临时字符串  保存处理了的字符串 
			if(currentlength == 1){//如果字符串长度为1  用下面的长度减一的话就没了 所以单独处理 
				temp = s.substr(currentlocal,1);
			}else{
				temp = s.substr(currentlocal, currentlength - 1);//提取子串  开始位置currentlocal  长度：因为要去除最后一个字符 所以长度-1	
			}
		
			result.push_back(temp);//把处理好的字符串存进去 
			
			currentlength = 1;//新的字符串 长度置为1 
			currentlocal = i;//起始位置为i 
		}
	}
	
	for(int i = 0; i < result.size(); i++){
		cout << result[i];//输出 
	}
	
	cout << endl;
	
}

```
---
---
## 输入两个字符串，输出这两个字符串的公共连续子序列
比如输入 abcdfef, bbcdfgh，输出 bcdf

	暴力解法：
	枚举第一个字符串的所有子串
	判断这个子串是否出现在第二个字符串里
	记录最长的
	**记录最长的是最值重要的**
	**因为只输出一个序列**

	求子串 a.substr(开始位置，长度)
```c

#include <iostream>
#include <string>
using namespace std;

int main(){
	string a, b;
	cin >> a >> b;
	string result = "";
	
	for(int  i = 0; i < a.length(); i++){//i表示遍历到a的位置 
		for(int len = 1; i + len <= a.length(); len++){//len表示 提取的子串长度   i+len不能超过a.length() 
			string str = a.substr(i , len);//提取子串函数 a.substr(i,len) 
			
			if(b.find(str) != string::npos && str.length() > result.length()){//能在b里找到子串  且长度更长 
			//在b里找子串 b.find(str) != string::npos 
				result = str;
			}
		}
	}
	
	cout << result <<endl;
	
}
```
---
---
## 输入整数 n，输入 n 个字符串，对这 n 个字符串降序排序后输出
#### 自定义降序排序函数  + sort


```c

bool cmp(string a, string b){
	return a > b;
}

sort(s.begin(), s.end(),cmp);
```
```c
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

bool cmp(string a, string b) {
    return a > b;  // 降序
}

int main() {
    int n;
    cin >> n;

    vector<string> s(n);

    for (int i = 0; i < n; i++) {
        cin >> s[i];
    }

    sort(s.begin(), s.end(), cmp);

    for (int i = 0; i < n; i++) {
        cout << s[i] << endl;
    }

    return 0;
}
```
---
---
## 输入一个字符串，以回车结束，将其按单词反序输出，例如输入 I am a student，输出 student a am I
```c
先读入一整行字符串  判断最后一个字符是否是标点
再把字符串输入stringstream 让它分隔字符
然后用栈输出  输出一个单词判断栈是否为空  空的话就是最后一个单词 若有标点 输出标点  否则输出单词+空格

stringstream：
#include<sstream>

stringstream ss(str)//把str变成字符流 给了ss
ss >> words  就像cin一样读入字符串



判断是否为英文标点符号
#include<cctype>
ispunct(flag);

str去除最后一个字符
str.erase(str.length()-1)

str.erase(pos);        // 删除pos位置开始到末尾
str.erase(pos,len);    // 删除len个字符
str.erase(str.length()-1); // 删除最后一个字符

```
```c
#include <iostream>
#include <sstream>
#include <string>
#include <stack>
#include <cctype>
using namespace std;

int main(){
	
	string str;
	getline(cin, str);//先读入一整行字符 以便于判断最后一个字符是不是标点 
	char endflag = 0;//标点 
	
	if(!str.empty()){//读入的不是空串 
		endflag = str[str.length()-1];//标点在末尾 
		
		if(ispunct(endflag)){//如果是标点 
			str.erase(str.length()-1);//str擦除最后的标点 
		}
	}
	
	stringstream ss(str);//输入流 
	//这里是括号
	//类型名字是stringstream
	
	stack<string> stk;
	
	string word;
	
	while(ss >> word){//读入 
		stk.push(word);//压入栈中 
	}
	
	while(!stk.empty()){
		cout << stk.top();
		stk.pop();
		
		if(stk.empty()){//栈空 说明是最后一个单词  
			if(endflag != 0) cout << endflag << endl;//如果有标点  输出标点 
		}else{
			cout << " ";//栈不空 说明是普通单词 输出空格 
		}
	}
	
}
```
---
---
```c
string s = "hello";

// insert：在指定位置插入
s.insert(5, " world");     // hello world
s.insert(0, 3, '*');       // ***hello world

// replace：替换指定位置的一段
s.replace(0, 3, "Hi");     // Hihello world
s.replace(2, 5, "ABC");    // HiABC world
--------------------------------------
str.insert(pos, "字符串");        // 插入
str.insert(pos, n, '字符');       // 插入n个字符
str.replace(pos, len, "字符串");  // 替换
str.replace(pos, len, n, '字符'); // 替换成n个字符
```
---
---
## 输入一个字符串，以回车结束，将字符串中的空格用 %20 填充，输出填充完成后的字符串
```c
%20 其实是 URL 编码（URL Encoding）里的表示方式。

在 URL 中：
空格  →  %20

原因是：
在网页地址（URL）里 空格不能直接出现，所以要用 ASCII 码的十六进制表示。

//输出字符串  遇到空格 输出“%20”就行了
for(int i=0;i<str.length();i++){
        if(str[i]==' ')
            cout<<"%20";
        else
            cout<<str[i];
    }
```
---
---
## 函数 search 在字符串 s 中查找子串 t，返回子串 t 在 s 中的首地址。若未找到，则返回 NULL。
```c
暴力解决就行 每次提取t长度的子串str 判断str和t是否相等 
#include <iostream>
#include <string>
using namespace std;

int ifsub(string s, string t, int a, int b){
	
	for(int i = 0; i + b <= a; i++){//结束条件 i + b <= a
		string str = s.substr(i, b);//s.substr(位置，长度） 
		if(str == t){
			return i;
		} 
	}
	return 0;
}

int main(){
	string s, t;
	cin >> s >> t;
	
	int a = s.length();
	int b = t.length();
	int flag = ifsub(s, t, a, b);
	
	if(flag == 0) cout << "NULL" << endl;
	else cout << flag << endl;
}
```
---
---
## 输入一个以 # 结束的字符串，本题要求滤去所有的非十六进制字符（不分大小写），组成一个新的表示十六进制数字的字符串，然后将其转换为十进制数后输出。如果在第一个十六进制字符之前存在字符 -，则代表该数是负数。
```c
读入一个以 # 结束的字符串：
 while(cin >> ch && ch != '#')
    {
        s += ch;
    }

判断是否为负数 标记是否出现了十六进制数 如果遇到了负号 但还没有出现十六进制数 说明结果是负数

十六进制转十进制：
long long sixtoten(string s){//将十六进制数转换成十进制数 
	
	long long result = 0;//一定要初始化
	
	for(int i = 0; i < s.length(); i++){
		
		result *= 16;//十六转十 result *= 16  将上次运算的结果*10 再加这次运算的结果 
		
		if(s[i] >= '0' && s[i] <= '9') result += s[i] - '0';
		if(s[i] >= 'a' && s[i] <= 'f') result += s[i] - 'a' + 10;//字母 
		if(s[i] >= 'A' && s[i] <= 'F') result += s[i] - 'A' + 10;
		
	}
	
	return result;
} 

```
```c
#include <iostream>
#include <string>
using namespace std;

int ifnum(char c){//判断是否是十六进制数 
	
	if((c >= '0' && c <= '9') || (c >= 'a' && c <= 'f') || (c >= 'A' && c <= 'F')){
		return 1;
	}
	
	return 0;
}

long long sixtoten(string s){//将十六进制数转换成十进制数 
	
	long long result = 0;//一定要初始化
	
	for(int i = 0; i < s.length(); i++){
		
		result *= 16;//十六转十 result *= 16  将上次运算的结果*10 再加这次运算的结果 
		
		if(s[i] >= '0' && s[i] <= '9') result += s[i] - '0';
		if(s[i] >= 'a' && s[i] <= 'f') result += s[i] - 'a' + 10;//字母 
		if(s[i] >= 'A' && s[i] <= 'F') result += s[i] - 'A' + 10;
		
	}
	
	return result;
} 


int main(){
	string str = "";
	char c;
	
	while(cin >> c && c != '#'){//读入字符 直到读入的字符是# 
		str += c;
	}
	
	int ifhex = 0;//标记是否出现过十六进制数 
	int flag = 1;//负号 
	string change = "";//保存新的十六进制字符串 
	
	for(int i = 0; i < str.length(); i++){
		if(ifnum(str[i])){//是十六进制数 
			ifhex = 1;//把标记改为1 
			change += str[i];//这个数计入change中 
		}
		if(str[i] == '-'){//出现负号 并且之前没有出现过十六进制数 
			if(ifhex == 0) flag = -1;//说明是负数 
		}
	}
	
	long long result = sixtoten(change) * flag;//十进制数 

	cout << result << endl;	
	
}
```
---
---
# 递归
---
## 用户输入一个字符串，对字符串中相同的字符进行去重，并且将字符串中的所有字符倒序输出，要求用递归的方法。
---
```c
#include <iostream>
#include <set>
#include <string>
using namespace std;

//这道题的递归 不是用在去重上  而是用在逆序输出上
//从后往前遍历 输出元素 然后递归调用   结束条件是 x<0 
void reverse(string l, int x){
	if(x < 0) return;
	cout << l[x];
	reverse(l, x-1);
}

int main(){
	string str, l;
	cin >> str;
	
	set<char> s;
	
	//去重 把元素放进s里 
	for(int i = 0; i < str.length(); i++){
		char c = str[i];
		s.insert(c);
	}
	
	//把set s里的元素放到string l里面  方面后面reverse函数的编写 
	for(set<char>::iterator it = s.begin(); it != s.end(); it++){
		l += *it;//string可以用 + 连接字符 
	}
	
	reverse(l, l.length() - 1);
	
}
```
---
#### string用法
```c
cin >> s不能读取空格

如果想读带空格的字符串  getline(cin,s);   
			或者gets(s); gets碰到换行符结束读入

find()	能查找子串/字符串
	string s = "hello";
	if(s.find("ll") != string::npos)
	{
		cout << "找到";
	}


什么是 string::npos？
意思是：  没找到

s.empty();
s.clear();
s.popback();//删除最后一个字符

str += c;//拼接字符串
```
---
---

## 递归写字符组合
---
### 设有字母a, b, c，
### 请编写程序用递归的方法产生由这些字母组成的，且
### 长度为n的所有可能的字符串及字符串个数。
### 要求必须用递归，否则不得分。
---

```c
#include <iostream>
#include <string>
using namespace std;

int n;//全局变量  这样函数里就能少写一个参数 

void f(string res){
	//res是值传递 每个res都是独立的字符串 

	if(res.length() == n){
		cout << res << endl;
		return;
	}
	
	f(res+'a');
	f(res+'b');
	f(res+'c');

}

int main(){
	
	cin >> n;
	
	string res = "";
	f(res);
}
```
---
## 用递归的方法求字符串的长度
```c
#include <stdio.h>
#include <stdlib.h> //引入 stdlib.h  可以使用malloc 

//递归函数
int f(char* s, int n){
	if(*s == '\0') return n;
	else return f(s + 1, n + 1);//s+1 读取下一个字符    n+1表示长度的累加 
}

int main(){
	char* s = (char*)malloc(100*sizeof(char)) ;
	//malloc函数   (char*)malloc(100*sizeof(char)) 
	//必须分配内存 要不然 s是一支野指针
	//要分配100个大小 要不然必然溢出
	 
	 //也可以用char s[100]替代 
	 
	  
	gets(s);//输入字符串 
	printf("%d", f(s,0));//长度从0开始累加
} 
```
---

## 用递归的方法对用户输入的字符进行全排列，例如：输入 ab，输出 ab，ba，输入 abc，输出 abc，acb，bac，bca，cab，cba。(排列的单词 permute)

```c
#include <iostream>
#include <string>

using namespace std;

void permute(string &str, int begin, int end){//str可能被改变 所以函数参数类型是string &str
//这里 string &str 加 & 的原因是：递归过程中需要修改同一个字符串，并且再恢复（回溯）。
//如果不用 &，程序仍然能运行，但每次递归都会复制一份字符串，效率更低，而且“交换再换回来”的意义就不明显了。
//所有递归共享同一个 str
 
	if(begin == end) {//如果排列到最后一位了 就可以输出 
		cout<<str<<endl;
		return;//返回 
	}
	
	for(int i = begin; i <= end; i++){//排列begin到end的元素 
		//begin表示排列到第几个字符 
		//让第i个位置上的元素来替换begin的位置 
		swap(str[begin], str[i]); //i 用来挑选哪个字符来放到 begin 这个位置
		permute(str, begin + 1, end);//递归 排列begin+1 到 end的元素 
		swap(str[begin], str[i]);//一定要交换回来 ！ 
		
	}
}

int main(){
	string str;
	cin>>str;
	permute(str, 0, str.length() - 1);//全排列从0到 str.length() - 1的元素 
	return 0;
}

```
---
---
## 递归计算 -1/4 + 1/8 - 1/12 + 1/20
结束条件是当n等于1    
此时返回 1/4

	类型		含义
	long long	整数类型
	double		浮点数（小数）

	long long a = 3.14;   // 实际存的是 3
	double b = 3.14;      // 存的是 3.14
long long 不能算小数

这道题 注意除以 4.0 
```c
#include <iostream>
using namespace std;

double f(int n){
	if(n == 1) return -(1/4.0);//结束条件是当n等于1
	if(n % 2 == 0) return f(n-1) + 1/(4.0 * n);//返回f(n-1) + 自己位置的结果
	else return f(n-1) + (-1)/(4.0 * n);
}

int main(){
	int n;
	cin >> n;
	cout << f(n) << endl;
}
```
---
---
## 输入一个数 n，递归输出表达式（可以理解为一颗二叉树的先序遍历）
例：输入 5，输出：
f(5)=f(4)+f(3),f(4)=f(3)+f(2),f(3)=f(2)+f(1),f(2)=1,f(1)=1,f(2)=1,f(3)=f(2)+f(1),f(2)=1,f(1)=1,<回车>
答案解析：设 F(n) 为打印 f(n) 的值，只有 n=1 和 n=2 时输出不同，作为** 递归边界 **即可。
```c
万能模板
void f(int n){

    // 1 终止条件
    if(n==1){
        cout<<"f(1)=1,";
        return;
    }

    if(n==2){
        cout<<"f(2)=1,";
        return;
    }

    // 2 输出当前节点
    cout<<"f("<<n<<")=f("<<n-1<<")+f("<<n-2<<"),";

    // 3 左子树
    f(n-1);

    // 4 右子树
    f(n-2);
}
```
---
```c
#include <iostream>
using namespace std;

void F(int n){
	if(n == 1) {
		cout << "f(1)=1" <<" ,";
		return;
	}
	if(n == 2) {
		cout << "f(2)=1" <<" ,";
		return;
	}
	//其他情况 先输出 再递归计算 
	printf("f(%d)=f(%d)+f(%d),",n, n-1, n-2);
	//符合二叉树的先序遍历   先左子树 再右子树 
	F(n-1);
	F(n-2);
}

int main(){
	int n;
	scanf("%d", &n);
	F(n);
	cout << endl;
	return 0;
}
```
---
---
## 构造函数 gcd(int x,int y),递归实现求两数的最大公约数
例如 gcd(56,98)
输出结果如下: gcd(56,98),gcd(98,56),gcd(56,42),gcd(42,14),gcd(14,0),gcd=14
例如 gcd(1,0)
输出结果如下: gcd(1,0),gcd=1

```c
#include <iostream>
using namespace std;

void gcd(int a, int b){
	
	if(b == 0){
		printf("gcd(%d,%d),",a, b);
		printf("gcd=%d",a);
		return;
	}
	if(a < b){
		printf("gcd(%d,%d),",a, b);
		swap(a,b);
	}
	
	printf("gcd(%d,%d),",a, b);
	gcd(b, a%b);
	
}

int main(){
	
	int a, b;
	cin >> a >> b;
	gcd(a,b);
	
}
```
---

---
 #### 有一串序列 1，1，2，3，5，8，13……，输入n，求这个序列的前n项和的值，例如n=1时，输出1，n=3时候输出4，n=4时，输出7；
```c
递归函数正常写 在主函数里用for循环里 sum += f(n)
```
---
---
## 输入整数n，输入有n行的杨辉三角形
比如输入4，输出：
```c
     1
    1 1
   1 2 1
  1 3 3 1
 1 4 6 4 1 
  ```
 先在数组里放好数字 再输出三角形状

 ```c
 只算数字
#include <stdio.h>

int main(){
	int a[250][250];
	int n;
	scanf("%d", &n);
	
	a[0][0] = 1;//第一行就一个元素 
	
	for(int i = 1; i <= n; i++){
		a[i][0] = 1;//每一行的第一个和最后一个是1 
		a[i][i] = 1;

		for(int j = 1; j < i+1; j++){
			a[i][j] = a[i-1][j-1] + a[i-1][j];//a[i][j] = a[i-1][j-1] + a[i-1][j]
		}
	}
	
	for(int i = 0; i <= n; i++){
		for(int j = 0; j <= i; j++){
			printf("%d",a[i][j]);
		}
		printf("\n");
	}
} 
 ```
```c
结果是
1
11
121
1331
14641
```
按有空格的输出
```c
#include <stdio.h>

int main(){
	int a[250][250];
	int n;
	scanf("%d", &n);
	
	a[0][0] = 1;//第一行就一个元素 
	
	for(int i = 1; i <= n; i++){
		a[i][0] = 1;//每一行的第一个和最后一个是1 
		a[i][i] = 1;

		for(int j = 1; j < i+1; j++){
			a[i][j] = a[i-1][j-1] + a[i-1][j];//a[i][j] = a[i-1][j-1] + a[i-1][j]
		}
	}
	
	for(int i = 0; i <= n; i++){
		for(int j = 0; j < n - i; j++) printf(" ");
		for(int j = 0; j <= i; j++) printf("%d ",a[i][j]);
		printf("\n");
	} 
} 

输出
4
    1
   1 1
  1 2 1
 1 3 3 1
1 4 6 4 1
```
---
---
## 输入k，n（1<=k<=9，1<=n<=9），对k、n求和sum=k+kk+kkk+……（n个k），如输入k=2，n=3，sum=2+22+222=246；
```c
递归结束条件 n==0 返回k
其他情况返回 f(n-1)*10 + k
```
---
---
## 输入一个字符串，用递归方式计算该字符串的长度
例如输入abcd，输出4


思路：参数是s的第n个字符 如果这个字符是'\0'  返回0  否则——__返回1+f(s[n+1])__
```c
int strlen_rec(char s[]){
    
    if(s[0] == '\0')
        return 0;

    return 1 + strlen_rec(s+1);
}
```
---
---
## 输入一个字符串，这个字符串中的不同的字符组成一个集合，输出这个集合的所有子集，包括空集
例如输入aabc，输出（），（a），（b），（ab）
__求子集 先去重__
```c
思路：
1.字符串去重  //可以用set  也可以自己写去重函数
2.让字符串进入递归函数
	每次处理一个字符 这个字符选择在还是不在 
	选择完之后让目前的结果字符串递归去选择下一个字符来还是不来
```
```c
#include <set>
#include <iostream>
#include <string>
using namespace std;

int n;//n是全局变量 在主函数中赋值  


/*
函数参数含义

current：当前已经选择的字符组成的子集  初始值为空 ""
随递归变化，每次“选当前字符”就会加上 str[i]

i：当前处理集合 str 的下标 初始值为 0
每次递归 i 都 +1，表示处理下一个字符

str：去重后的字符集合
例如输入 aabc → 去重 → str = "abc"

n：集合长度，即 str.length()*/

//求子集的递归函数 
void ziji(string current, int i, string str){
	if(i == n){//已经选到了最后一位 
		cout << "(" << current << ")" << endl ;//current表示目前选择的字符 
		return;
	}
	
	ziji(current+str[i], i+1, str);//当前字符选择进去 
	ziji(current, i+1, str);//不进去 
	
}

int main(){
	string s;
	cin >> s;
	set<char> vase;
	
	for(int i = 0; i < s.length(); i++){
		vase.insert(s[i]);//去重 
	}
	
	string str ="";
	//把vase想字符放到字符串里 
	for(set<char>::iterator it = vase.begin(); it != vase.end(); it++){
		str += *it;
	}
	//n表示去重后的字符串的长度 
	n = str.length();
	//current表示选择字符   或者 current就代表子集 
	string current = "";
	
	//从str的第0位开始运算 
	ziji(current,0,str);
}
```
---
---
## 汉诺塔问题
汉诺塔问题图示
将A杆上的盘子全部移动到C杆上需要移动几步，要求始终都保持大盘在下小盘在上的样子，输入n，表示A杆上有n个盘子，例如输入1，输出1，输入2，输出3。
```c
总共的移动次数求法：
将前n-1个盘子移到中转站 再将最后一个盘子移到目标站  再将n-1个盘子移到目标站
所以 f(n) = f(n-1) + 1 + f(n-1)
f(n) = 2*f(n-1) + 1
递归结束条件 n==1	f(1) = 1
```
```c
打印移动顺序：
#include <iostream>
using namespace std;

//n代表需要移动的盘子的个数 
//from代表起始位置
//temp代表中转辅助位置
//to代表目标位置 
void hanoi(int n, char from, char temp, char to){
	
	//递归的结束条件  起始位置只有一个盘子  直接把它从起始位置移动到目标位置 
	if(n == 1){
		cout << from << "->" << to << endl;//输出移动过程 
		return;
	}
	
	hanoi(n-1, from, to, temp);//先把前n-1个盘子从起始位置借助目标位置  移动到辅助位置 
	
	hanoi(1,from,temp,to);//把剩下的最后一个大盘子直接移动到目标位置 
	
	hanoi(n-1, temp, from, to);//把n-1个盘子 借助起始位置 从临时位置移动到目标位置 
	
}

int main(){
	int n;
	cin >> n;
	hanoi(n, 'a', 'b', 'c');//起始位置 中转 目标 的名字是abc 
	/*
	from = a   起始柱
	temp = b   中转柱
	to   = C   目标柱
	*/
}

```
```c
#include <iostream>
#include <string>
using namespace std;

void f(int n, string begin, string mid, string end){
	
	if(n == 1){
		cout << begin << "->" << end << endl;
		return;
	}
	
	else{
		
		f(n-1, begin, end, mid);
		
		f(1, begin, mid, end);
		
		f(n-1, mid, begin, end);
		
	}
}


int main(){
	
	int n;
	cin >> n;
	
	f(n, "a", "b", "c");
}


```
---
---
#### 递归求阶乘的和 定义sum类型为long long 因为结果会超出Int 的范围
---
---
## 亚东上课需要走n阶台阶，因为他腿比较长，所以每次可以选择走一阶或者走两阶，那么他一共有多少种走法
```c
结束条件 	最后剩下 两级台阶（返回2）或者一级台阶（返回1）
递推条件	f(n) = f(n-1) + f(n-2)	
//f(n-1)表示 最后一步上了一级台阶 算前面n-1级的个数
//f(n-2)表示 最后一步上了两级台阶 算前面n-2级的个数
```
```c
#include <stdio.h>

int f(int n){
	
	if(n == 1) return 1;
	if(n == 2) return 2;//两级台阶有两种走法 
	
	return f(n-1) + f(n-2);//n级台阶 最后可能剩下1/2级  剩下一级->求前n-1级的个数  剩下2级 求前n-2级的个数 
}

int main(){
	int n;
	scanf("%d", &n);
	int sum = f(n);
	printf("%d", sum);
}
```
---

---

# 数学
---

__保留八位小数的输出：__
```c
 %.8lf
```
 __sum初始化为0.0__
 __while(1){//用while循环 不符合条件时退出 可以防止最后一项被误加进__

---
## 计算（-1）^(n+1) * (4*n - 3) 累次计算 直到直到最后一项小于 输入的精度
```c
#include <stdio.h>
#include <math.h>

int main(){
	double x;
	scanf("%lf", &x);
	double sum = 0.0;//对sum初始化为0.0 
	int flag = 1;
	int n = 1;//从第一项开始 

	//最规范的写法
	while(1){//用while循环 不符合条件时退出 可以防止最后一项被误加进去
		//1.计算项数的值 
		double term = flag * (4.0 / (4.0 * n - 3.0));
		//2.不符合条件就break 
		if(fabs(term) < x) break;//用fabs取绝对值 避免因为term是负数 而退出 
		//3.先把term加进去 
		sum += term;
		//4.再为下一次计算做准备 
		flag = -flag;
		n++;
		
	}
	
	printf("%d\n", n -1); 
	printf("%.8lf\n", sum);
	
}
```
---

## 一种数 它的平方的右侧等于它本身 比如25 = 625的右侧 找出 0-10000之间所有的这种数
__求数的位数时   对0单独处理__

```c
#include <stdio.h>
#include <math.h>//math函数  计算pow 

int fd(int x){//判断一个数的位数的函数 
	int d = 0;
	
	if(x == 0) return 1;//对0 单独处理  ；而且对0的处理要放在while循环前面  因为所有的x最后都会变成0 
	
	while(x > 0){//不能 写 x>=0  因为会出现死循环 
		x = x/10;
		d ++;
	}
	
	return d;
}

int main(){

	for(int x = 0; x <= 10000; x++){
		
		int powx = pow(x, 2);
		int nten = pow(10, fd(x));//nten表示10的d次方 
		
		//powx 对nten取余数 代表“右边的数” 
		if(powx%nten == x) printf("%d\n", x);
		
	}
	
	return 0;
}
```

---
---
## 进制转换 输入一个数n和一个十进制数s 把s转换为n进制数
---
1.思路：
s % n   ← 取余
s / n    ← 更新数
直到：s == 0

2.使用栈来实现逆序输出

__3.小于10的数直接存入
	大于10的数要变成A B C (x - 10 + 'A')__

```c
#include <iostream>
#include <stack>
using namespace std;

int main(){
	int n, s;
	cin >> n >> s;//读入 数字之间以空格间隔 
	stack <char> stk;//存入字符char  因为a b c是字符  前面0-9 也用字符存 
	
	while(s != 0){//结束条件 s==0 
		char c;//c表示要存入的字符 
		if(0 <= s%n && s%n <=9) {//取余数之后在0-9之间 
			c = s%n + '0';
			stk.push(c);
		}
		else{
			c = s%n - 10 + 'A';//大于等于10   c = s%n - 10 + 'A'
		}
		s = s/n;
		
	}
	
	while(!stk.empty()){//非空 一直输出 
		cout << stk.top();
		stk.pop();
	}
	
	cout<<endl;
	
}
```
---
---
## 给定一个数 n，输出所有 a+b+c=n，其中 c>b，b>a例：如输入 9，输出1+2+6=9,1+3+5=9,2+3+4=9<回车>
vector 就是“可以自动变长的数组”
__一次循环要记录下多个数 考虑用数组的数组__

__vector<vector<int>> result;
意思是： 创建一个 存放“整数数组”的 vector__




读取——
result[i][0]  // a
result[i][1]  // b
result[i][2]  // c

```c
#include <iostream>
#include <vector>
using namespace std;

int main(){
	int n;
	cin >> n;
	
	//vector是一个大小可变的容器 
	/*	result里的元素  
	[
 		[1,2,6],
 		[1,3,5],
 		[2,3,4]
	]*/
	vector<vector<int> > result;//两个  >> 中间用空格分开 老编译器会认为连着的两个大于号是 输入福
	
	for(int a = 1; a <= n - 2; a++){
		for(int b = a+1; b <= n - 1; b++){
			for(int c = b+1; c <= n; c++){
				if(a + b + c == n){
					//旧版本不支持一下存放进result   所以先把abc存入temp容器  再把temp存入reuslt 
					vector<int> temp;
					temp.push_back(a);//是push_back  不是pushback 
					temp.push_back(b);
					temp.push_back(c);
					result.push_back(temp);
				}
			}
		}
	}
	
	//result.size() 
	for(int i = 0; i < result.size(); i++){
		//result[i][0], result[i][1], result[i][2]
		printf("%d+%d+%d=%d",result[i][0], result[i][1], result[i][2], n);
		if(i != result.size() - 1) cout << "," ;//不是最后一个元素 输出逗号 
	}
	
	cout << endl;//最后一个元素后面输出换行符 
	
}
```
---
## 输入两个正整数 m,n(1<m<9,1<n<9),构造一个二维数组(下表从1开始),每个元素的值是行和列的乘积,输出二维数组边界矩形元素的和。
不合法输出"input error." 例如:m=3,n=4.
输出结果如下: 50
```c
#include <iostream>
#include <vector>
using namespace std;

int main(){
	int m, n;
	cin >> m >> n;
	vector<vector<int> > result;//因为不能通过m n来定义数组 所以用vector<vector<int>> 来保存数据 
	int sum = 0;
	
	for(int i = 0; i < m; i++){
		vector<int> temp;//临时数组 存放这一行的数据 
		for(int j = 0; j < n; j++){
			int num = (i + 1)*(j + 1);
			temp.push_back(num);//temp储存每个数据 
		}
		result.push_back(temp);//把整行数据组成的数组 放进vec里 
	}
	
	for(int i = 0; i < m; i++){
		if(i == 0 || i == m - 1){//第一行和最后一行的数据都加进去 
			for(int j = 0; j < n; j++){
				sum += result[i][j];
			}
		}
		else{
			for(int j = 0; j < n; j++){
				if(j == 0 || j == n - 1){//其他行的数据 只加进去第一个和最后一个数据 
					sum += result[i][j];
				}
			}
		}
	}
	
	cout << sum;
}
```
---
---
## 最大公约数 最小公倍数
```c
#include <stdio.h>

int gcd(int a, int b){
	return (b == 0) ? a : gcd(b, a%b);//用三目运算符 
}

int main(){
	int a, b;
	scanf("%d%d",&a, &b);
	if(a < b){
		int temp = a;
		a = b;
		b = temp;
	}
	printf("%d",gcd(a, b));
}
```
__最小公倍数 两数乘积 / 最小公倍数__

---
---
## 输入两个非常大的正数  范围超过long  输出这两个数的和 
思路：
把两个大数 转成string类型
从两个字符串的末尾开始处理 这一位的结果是两数相加+进位%10  
result = 结果 + result  || 这个数要拼在result的前面
更新进位为两数相加+进位/10

__无论num是否大于10  都要计算进位 不是只有在num大于10时 才计算__

```c
#include <iostream>
#include <string>
using namespace std;

int main(){
	string a, b;
	cin >> a >> b;
	
	//result刚开始定义为空串 
	string result ="";
	int carry = 0;
	
	int i = a.length() - 1;//i代表处理的a的位置  从个位开始处理 
	int j = b.length() - 1;
	
	while(i >= 0 || j >= 0 || carry > 0){//因为a b长度不一定相同  并且ab处理完 进位可能还有数字  所以这三个条件任意一个满足都可进入循环 
		
		int x = (i >= 0) ? a[i] - '0' : 0;//当i还能表示第几位时  用x 表示这个位置上的数字  否则置为0来参与运算 
		int y = (j >= 0) ? b[j] - '0' : 0;
		
		int num = (x + y + carry) % 10;//这个位置的结果 
		
		result = char(num + '0') + result;//先类型转换 再拼接   要拼在result前面 
		
		carry = (x + y + carry) / 10;//重置进位 
		
		i--;//每次让位置往前移   向高位移动 
		j--;
		
	}
	
	cout << result << endl;
	
}
```
---
---
## 将十进制数转换成七进制数
```c
#include <iostream>
#include <string>
#include <stack>
using namespace std;

int main(){
	int n;
	cin >> n;
	stack<int> stk;
	
	//n=n/7 压入n%7 最后逆序输出 
	while(n > 0){
		stk.push(n % 7);
		n = n/7;
	}
	
	//用栈逆序输出 
	while(!stk.empty()){
		cout << stk.top();
		stk.pop();
	}
	
}
```
---
---
---
#### 如果使用 vector<int> vec; vec[i]

#### 一定要先：vector<int> vec(n)     ----- 	定义vec的大小 
---
---
## 输入n个数字 以回车结束 若有重复数字 只保留一个 并且最后按从大到小排
情况一： 输入数字  且数字之间以空格分开 
```c

#include <iostream>
#include <set>
#include <stack>
using namespace std;

int main(){
	
	set<int> s;
	int x;
	stack<int> stk;
	
	//读入数字 直到遇到换行符 
	while(cin >> x){
		
		s.insert(x);
		
		if(cin.peek() == '\n') break;
	}
	
	//逆序排序  使用stack 
	for(set<int>::iterator it = s.begin(); it != s.end(); it++ ){
		stk.push(*it);
	}
	
	while(!stk.empty()){
		cout << stk.top();
		stk.pop();
	}
	
}
```
情况二 一次性输入n个数字
用string保存这串数字
```c
#include <iostream>
#include <set>
#include <stack>
#include <string>
using namespace std;

int main(){
	
	set<char> s;
	string str;
	stack<char> stk;
	
	cin >> str;
	
	//set去重 
	for(int i = 0; i < str.length(); i++){
		s.insert(str[i]);
	}
	
	//逆序排序  使用stack 
	for(set<char>::iterator it = s.begin(); it != s.end(); it++ ){
		stk.push(*it);
	}
	
	while(!stk.empty()){
		cout << stk.top();
		stk.pop();
	}
	
}
```
---

---
##  已知 2008 年 1 月 1 日是周二，输入年月日，输出这一天是礼拜几和这一天是这年的第几天
第几天：按照数组算 根据是否为闰年 写出月份数组的值 然后计算是今年的第几年
星期几：算和2008.1.1的天数差 然后对7取余数 就是礼拜几
```c
/*已知 2008 年 1 月 1 日是周二，输入年月日，输出这一天是礼拜几和这一天是这年的第几天*/
#include <iostream>
using namespace std;

//判断是否为闰年 
int lunaryear(int x){
	
	if((x % 4 == 0 && x % 100 != 0) || x % 400 == 0){//闰年的判断方式 
		return 1;
	}
	
	return 0;
	
}

int main(){
	
	int y, m, d;
	cin >> y >> m >> d;
	
	int days[13] = {0, 31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};//每个月多少天的数组
	
	if(lunaryear(y)) days[2] = 29;//如果是闰年 二月的天数变为29
	
	int sum = 0;//计算这个月之前的天数 
	for(int i = 1; i < m; i++){
		sum += days[i];
	} 
	sum += d;//加上这个月的天数 
	
	cout << "days are " << sum <<endl;
	
	int count = 0;//计算和2008.1.1的距离
	
	for(int i = 2008; i < y; i++){
		if(lunaryear(i)) count += 366;
		else count += 365;
	} 
	
	count += sum - 1;//加上今年的天数  减去1 是因为1.1也是一天
	 
	int diff = (count+2)%7;//加二是因为2008.1.1 是星期二  
	
	
	//diff==0 是星期天  
	string name[7] = {"sunday", "monday", "tus", "thr", "wes", "fri","thi"};//补上星期的名字数组 
	cout <<  name[diff] << endl;
}
```
---

---
## 输入整数 n，生成一个这样的正方形
	如输入 5，输出
	1   2   3   4   5
	16  17  18  19  6
	15  24  25  20  7
	14  23  22  21  8
	13  12  11  10  9
---
	思路
	每一圈按顺序走：
	→ 从左到右
	↓ 从上到下
	← 从右到左
	↑ 从下到上

	走完一圈后 边界缩小。

__标记现在到几的数字num起名不要是i__
```c
#include <iostream>
using namespace std;

int main(){
	int n ;
	cin >> n;
	int a[100][100];//定义好大小 
	
	int top = 0;//上边界 
	int bottom = n-1;//下边界 
	int left = 0;//左边界 
	int right = n-1;//右边界
	int num = 1;
	
	while(top <= bottom && left <= right){//只要没有越界就可以进行  等于也可以进行 只是重叠了 但没有越界 
		
		for(int i = left ; i <= right; i++){//上边界 从左到右放数 
			a[top][i] = num++;
		}
		top++;//上边界往下移 所以++ 
		
		for(int i = top; i <= bottom; i++){//右边界 从山往下放数 
			a[i][right] = num++;
		}
		right--;//右边界左移 
		
		for(int i = right; i >= left; i--){//下边界  从右往左放数 
			a[bottom][i] = num++;
		}
		bottom--;//下边界上移 所以-- 
		
		for(int i = bottom; i >= top; i--){//左边界 从下到上放数 
			a[i][left] = num++;
		}
		left++;//左边界右移 
	} 
	
	for(int i = 0; i <= n-1 ; i++){
		for(int j = 0; j <= n-1 ; j++){
			cout << a[i][j] << "\t";//""引住   制表符\t 
		}
		cout << endl;
	}
}
```
---

---
## 给定一个 n×n 的方阵，本题要求计算该矩阵除副对角线、最后一列和最后一行以外的所有元素之和。副对角线为从矩阵的右上角至左下角的连线。
```c
副对角线：i + j == n - 1

先读入n
然后定义一个vector<vector<int> > vec(n, vector<int>(n))   ！！！
读入  
    for(int i = 0; i < n; i++)
        for(int j = 0; j < n; j++)
            cin >> vec[i][j];

------
或者定义int a[100][100];
```
---
---
##  将 1 到 n×n 之间的每个整数，从 1 开始，按照顺序依次填入到 n 阶蛇形方阵。
	输入 5
	输出样例 
	1   2   6   7   15
	3   5   8   14  16
	4   9   13  17  22
	10  12  18  21  23
	11  19  20  24  25
```c
蛇形矩阵记住三句：
k = i + j
k 从 0 到 2n-2
偶数右上，奇数左下
```
__i j和为k__
__每次填数字 while循环的条件是 一个大于等于0 一个小于n__
```c
#include <iostream>
#include <cmath>
using namespace std;

int main(){
	int n;
	cin >> n;
	int num = 1;
	
	int a[100][100];//定义好数组大小  oj系统用a[100][100]就行了 
	
	for(int k = 0; k <= 2*n - 2; k++){//对角线上横纵坐标之和k 从   0 - 2n-2 
		if(k % 2 == 0){//偶数对角线向右上填写 
		
			//先确定最大行号  确定了最大行号 也就确定了最小列号 
			int i = min(n-1, k);//因为总共只有n号 所以行号不能超过n-1  行号在k和n-1中取最小 
			int j = k - i;//行列和为k 
			
			while(i >= 0 && j < n){//保证在矩阵内 行号越来越小 所以约束在>=0  列好越来越大   所以约束在<n 
				a[i][j] = num++;
				i--;//行号减小 
				j++;//列号增大 
			}
		}
		else{
			int j = min(k, n-1);
			int i = k - j;
			
			while(j >= 0 && i < n){
				a[i][j] = num++;
				i++;
				j--;
			}
			
		}
	}
	
	//输出  制表符\t 
	for(int i = 0; i < n; i++){
		for(int j = 0; j < n; j++){
			cout << a[i][j]  << "\t";
		}
		cout << endl;
	}
}
```
---
---
## 本题要求编写程序，将给定 n×n 方阵中的每个元素循环向右移 m 个位置，即将第 0、1、…、n-1 列变换为第 n−m、n−m+1、…、n−1、0、1、…、n−m−1 列
	reverse函数就行
	每一行先整个翻转 再翻转前m个 再翻转后n-m个

	reverse函数用algorithm引入
	reverse(开始位置，要反转的末尾的下一个位置)
```c
#include <iostream>
#include <algorithm>
using namespace std;

int main(){
	int n, m;
	cin >> n >> m;

	int a[100][100];
	
	//读入 
	for(int i = 0; i < n; i++){
		for(int j = 0; j < n; j++){
			cin >> a[i][j];
		}
	}
	
	for(int i = 0; i < n; i++){//i表示行号 而不是第几个元素 
		reverse(a[i],a[i]+n);//整体翻转 
		reverse(a[i],a[i]+m);//前m个 
		reverse(a[i]+m, a[i] +n);//后面的 
	}
	
	for(int i = 0; i < n; i++){
		for(int j = 0; j < n; j++){
			cout << a[i][j] << "\t"; 
		}
		cout << endl;
	}
	
}
```
---
---
# 链表
---


## 输入一串数字 中间以空格分开 最后以换行符结束 要求用链表实现 奇数从大到小输出 偶数从小到大输出
```c
#include <iostream>

using namespace std;

struct Node{//c++定义struct 
	int data;
	Node* next;
};
//尾插法
void insert(Node*& head, int x){//Node*& head 是主函数head指针的别名 不是别名 是同一个变量 
	Node* newnode = new Node();//Node*类型  new node()为newnode分配内存 
	// new 是c++的关键词  用于分配内存 
	newnode->data = x;
	newnode->next = NULL;
	
	if(head == NULL){//空链表 
		head = newnode;//如果是空链表  让新节点作为头结点 
		return;//函数结束 不需要进行下面的操作 
	}
	
	Node* p = head;
	while(p->next != NULL){//P遍历到最后一个节点   while条件是 p->next != NULL 
		p = p->next;
	}
	p->next = newnode;//让新节点连接在尾部 
	
}

void sortup(Node* head){//从小到大排序  函数参数是*Node 
	if(head == NULL) return;// 边界条件：空节点 
	
	Node* p;
	Node* q;
	//冒泡排序 
	for(p = head; p->next != NULL; p = p->next){//p是   p->next != NULL  而不是P++
		for(q = p->next; q != NULL; q = q->next){// q是 q != NULL
			if(p->data > q->data) swap(p->data, q->data);
		}	
	}
}

void sortdown(Node* head){//从大到小排序 
	if(head == NULL) return;
	
	Node* p;
	Node* q;
	
	for(p = head; p->next != NULL; p = p->next){
		for(q = p->next; q != NULL; q = q->next){
			if(p->data < q->data) swap(p->data, q->data);
		}	
	}
}

void print(Node* head){//打印函数 
	Node* p = head;
	while(p != NULL){//终止条件是p!=NULL
		cout<<p->data;//cout<<p->data 
		p = p->next;
	}
	cout<<endl;//换行 
}


int main(){
	Node* even = NULL;//定义两个头结点 先置为空    一定要置为空 否则不运行啊
	Node* odd = NULL;//定义用 Node*  
	int x;
	
	while(cin>>x){//while(cin>>x) 一直读入值 赋值给x 
		if(x%2 == 0)  insert(even,x);
		else insert(odd,x);
		
		//注意他们的顺序  读入x之后 先插入 再判断顶峰是不是换行符 否则顺序颠倒 最后一个数字会被吞掉

		// if(cin.peek() == '\n') break
		if (cin.peek() == '\n') break;//遇到换行符结束读入  
	}
	//排序 
	sortup(even);
	sortdown(odd);
	//打印 
	print(odd);
	print(even);
	
	return 0;
	
}
```
---
#### 输入一串数字 中间以空格分开 最后以换行符结束 的读入
```c
  	while(cin>>x){//while(cin>>x) 一直读入值 赋值给x 
		操作
		if (cin.peek() == '\n') break;//遇到换行符结束读入  
	}
```
---

### 直接对主函数里的头结点操作  函数里的参数应该写：
```c
Node*& head
```
---
### 为节点分配内存的函数
```c
Node* newnode = new Node();
```
---
### 尾插法
```c
void insert(Node*& head, int x){//Node*& head 是主函数head指针的别名 不是别名 是同一个变量 
	Node* newnode = new Node();//Node*类型  new node()为newnode分配内存 
	// new 是c++的关键词  用于分配内存 
	newnode->data = x;
	newnode->next = NULL;
	
	if(head == NULL){//空链表 
		head = newnode;//如果是空链表  让新节点作为头结点 
		return;//函数结束 不需要进行下面的操作 
	}
	
	Node* p = head;
	while(p->next != NULL){//P遍历到最后一个节点   while条件是 p->next != NULL 
		p = p->next;
	}
	p->next = newnode;//让新节点连接在尾部 
	
}
```
---
### 给链表的节点冒泡排序
```c
void sortup(Node* head){//从小到大排序  函数参数是*Node 
	if(head == NULL) return;// 边界条件：空节点 
	
	Node* p;
	Node* q;
	//冒泡排序 
	for(p = head; p->next != NULL; p = p->next){//p是   p->next != NULL
		for(q = p->next; q != NULL; q = q->next){// q是 q != NULL
			if(p->data > q->data) swap(p->data, q->data);
		}	
	}
}
```
---
---
## 输入若干数字，以回车结束，将它们以链表形式存储，并递归反序输出这些数字。
例如输入 1，2，3，4，输出 4，3，2，1

	如果输入的数字是以逗号间隔 读取数据
	char c = cin.peek();
	if(c ==  '\n') break
	if(c == ',') cin.get()
-------------------------------------
__cin.get()： 从输入流读取一个字符__



```c
int main(){

    int x;
    char c;
    Node* head = NULL;

    while(cin >> x){     // 读数字
        insert(head, x);

        c = cin.peek();  // 看下一个字符

        if(c == ',')     // 如果是逗号
            cin.get();   // 读掉逗号
        else if(c == '\n')
            break;
    }

    reverse(head);
}
```
```c

#include <iostream>
using namespace std;

struct Node{
	
	int data;
	Node* next;
	
};

void insert(Node*& head, int x){//插入元素 

	Node* newnode = new Node();
	
	newnode->data = x;
	newnode->next = NULL;
	
	if(head == NULL){
		head = newnode;
		return;
	}
	
	Node* p = head;
	
	while(p->next != NULL){
		p = p->next;
	}
	
	p->next = newnode;
	
}

void reverse(Node* head){//递归逆序输出 
	
	if(head == NULL) return;//递归结束条件 
	reverse(head->next);//先输出后面的节点 
	cout << head->data <<endl;//再输出自己 
	
}


int main(){
	
	int x;
	Node* head = NULL;
	
	while(cin >> x){
		
		insert(head, x);
		if(cin.peek() == '\n') break;
		
	}
	
	reverse(head);
	 
}
	

```
---
---
## 输入一串字符，以回车结束，将字符中的字母按从大到小的顺序存储在链表中，然后输入链表
```c

#include <iostream>
#include <string>
#include <algorithm>
using namespace std;

struct Node{
	
	char data;
	Node* next;
	
};

bool cmp(char a, char b){//自定义排序规则函数 
	return a > b;
}

void insert(Node*& head, char c){
	
	Node* newnode = new Node();
	newnode->data = c;
	newnode->next = NULL;
	
	if(head == NULL){
		head = newnode;
		return;
	}
	
	Node* p = head;
	
	while(p->next != NULL){
		p = p->next;
	}
	
	p->next = newnode;
	
}

void fprint(Node* head){
	
	Node* p = head;
	
	while(p != NULL){
		cout << p->data <<endl;
		p = p->next;
	}
	
}

int main(){
	
	string s;
	getline(cin , s);//读取一整行字符串 
	Node* head = NULL;//一定要初始化 ！！！ 
	
	sort(s.begin(), s.end(), cmp);
	//sort(起始位置，结束位置的下一个) 
	//string是s.begin()  s.end() （s.end()指向最后一个元素的下一个位置） 
	
	for(int i = 0; i < s.length(); i++){
		
		if(s[i] != ' ') insert(head, s[i]);//读取的字符串中可能有空格 过滤掉空格 非空格才插入节点 

	}
	
	fprint(head);
	
}
```
---
---
## 输入整数k，和n，输入k个整数，使用链表存储，输出链表的所有数字和链表中的第n个数
```c
/*输入整数k，和n，输入k个整数，使用链表存储，输出链表的所有数字和链表中的第n个数*/
#include <iostream>
using namespace std;

struct Node{
	
	int data;
	Node* next;
	
};

void insert(Node*& head, int x){
	
	Node* newnode = new Node();
	
	newnode->data = x;
	newnode->next=NULL;
	
	if(head == NULL){
		
		head = newnode;
		return;
		
	}
	
	Node* p = head;
	
	while(p->next != NULL){
		p = p->next;
	}
	
	p->next = newnode;
}

void fprint(Node* head, int n){
	
	Node* p = head;
	
	for(int i = 0; i < n; i++){
		p = p->next;
	}
	
	cout << p->data;
	
}

int main(){
	
	int k, n;
	
	cin >> k >> n;
	
	int x;
	Node* head = NULL;
	
	for(int i = 0; i < k; i++){
		cin >> x;
		insert(head, x);
	}
	
	fprint(head, n);
	
}
```
---
---
## 输入若干个整数，以回车结束，用链表存储这些数字，输入整数k，输出链表中的倒数第k个节点
（双指针 p走k个后 p.q同走）
```c
#include <iostream>
using namespace std;

struct Node{
	
	int data;
	Node* next;
	
};

void insert(Node*& head, int x){
	
	Node* newnode = new Node();
	
	newnode->data = x;
	newnode->next=NULL;
	
	if(head == NULL){
		
		head = newnode;
		return;
		
	}
	
	Node* p = head;
	
	while(p->next != NULL){
		p = p->next;
	}
	
	p->next = newnode;
}

void fprint(Node* head, int k){//双指针的方法输出倒数第n个节点 
	
	Node* p = head;
	Node* q = head;
	
	for(int i = 0; i < k; i++){//p先走k个 
		p = p->next;
	}
	
	while(p != NULL){//p q 同时走 p走到末尾 q走到倒数第k个 
		p = p->next;
		q = q->next;
	}
	
	cout << q->data;
	
}

int main(){
	
	int x;
	Node* head = NULL;
	
	while(cin >> x){
		insert(head, x);
		if(cin.peek() == '\n') break;
	}
	
	int k;
	
	cin >> k;
	
	fprint(head, k);
	
}
```
---
---
## 输入若干个整数，构造两条升序链表，一条存放奇数一条存放偶数，输出两条链表后将它们合并成第三条也为升序的链表。输出这第三条链表

下面的代码构建第三条链采用双指针的思路
```c
/*输入若干个整数，构造两条升序链表，一条存放奇数一条存放偶数，
输出两条链表后将它们合并成第三条也为升序的链表。输出这第三条链表*/
#include <iostream>
#include <algorithm>
using namespace std;

struct Node{
	
	int data;
	Node* next;
	
};

void insert(Node*& head, int x){
	
	Node* newnode = new Node();
	
	newnode->data = x;
	newnode->next=NULL;
	
	if(head == NULL){
		
		head = newnode;
		return;
		
	}
	
	Node* p = head;
	
	while(p->next != NULL){
		p = p->next;
	}
	
	p->next = newnode;
}

void fprint(Node* head){
	
	Node* p = head;
	if(p == NULL) return; //空链

	while(p != NULL){
		
		cout << p->data << " ";//先输出 
		p = p->next;//再往后移 
	}
	
}

void sortup(Node* head){//从小到大排序函数 
	
	Node* p = head;
	Node* q = p->next;
	
	for(p = head; p->next != NULL; p = p->next){//冒泡排序 
		for(q = p->next; q != NULL; q = q->next){
			if(p->data > q->data) swap(p->data, q->data);
		}
	}
	
}

void sort(Node*& result, Node* even, Node* odd){
	
	Node* p = even;
	Node* q = odd;
	
	while(p != NULL && q != NULL){
		
		if(p->data < q ->data){
			insert(result, p->data);
			p = p->next;
		}
		else{
			insert(result, q->data);
			q = q->next;
		}
		
	}
	
	if(p != NULL){//p还有剩余元素没有处理 
		while(p != NULL){
			insert(result, p->data);
			p = p->next;
		}
	}
	
	if(q != NULL){//q还有剩余元素没有处理 
		while(q != NULL){
			insert(result, q->data);
			q = q->next;
		}
	}
	
}

int main(){
	
	int x;
	Node* even = NULL;
	Node* odd = NULL;
	
	while(cin >> x){
		
		if(x%2 == 0)insert(even, x);
		if(x%2 == 1) insert(odd, x);
		if(cin.peek() == '\n') break;
		
	}
	
	sortup(even);
	sortup(odd);

	fprint(even);
	cout << endl;
	
	fprint(odd);
	cout << endl;
	
	Node* result = NULL;
	
	sort(result, even, odd);//对result的排序 采用双指针的思路
	fprint(result);
	cout << endl; 
	
}

```
---
---
## 输入若干个整数，以回车结束，用链表存储这些数，将链表中的重复节点删除后输出链表
	有序链表删除重复节点:
		当前节点 和 下一个节点比较
		如果相同 → 删除下一个节点
		如果不同 → 向后移动
```c

void fdelete(Node* head){//删除函数 用双指针 
	
	Node* p = head;
	
	while(p->next != NULL){
		
		Node* q = p->next;
		
		if(p->data == q->data){//如果下一个节点和当前节点一样 删除下一个节点 但不移动p 
			p->next = q->next;
			q->next = NULL;
			delete q;
		}
		else{
			p = p->next;//不一样 才移动p 
		}
	} 
	
}
```
------------------------
	无序链表删除重复节点
		每个节点
		和它后面的所有节点比较
```c
void removeDuplicate(Node* head){

    Node* p = head;

    while(p != NULL){

        Node* q = p;

        while(q->next != NULL){

            if(q->next->data == p->data){

                Node* temp = q->next;
                q->next = temp->next;
                delete temp;

            }
            else{
                q = q->next;
            }

        }

        p = p->next;

    }

}
```
---
---
## 输入若干个整数，构造一个双向链表，分别输出正向和反向的链表
head tail分别指向链表的头部 尾部

是双向链表 但不是双向循环链表
```c
#include <iostream>
using namespace std;

struct Node{
	
	int data;
	Node* next;
	Node* prev;//指向前面节点的指针prev 
	
};

void fprintbegin(Node* head){//从头开始打印 
	
	Node* p = head;
	if(p == NULL) return; //空链

	while(p != NULL){
		
		cout << p->data << " ";//先输出 
		p = p->next;//再往后移 
	}
	
}

void fprintend(Node* tail){//从尾部开始打印 
	
	Node* p = tail;
	if(p == NULL) return; //空链

	while(p != NULL){/
		
		cout << p->data << " ";//先输出 
		p = p->prev;//再往前移 
	}
	
}

//插入函数最主要的就是更新尾部 
void insert(Node*& head, Node*& tail, int x){//注意是 *& 
	
	Node* newnode = new Node();
	newnode->data = x;
	newnode->next = NULL;
	newnode->prev = NULL;
	
	if(head == NULL && tail == NULL){//如果是空表 那么 头和尾都指向刚插入的这个节点 这个节点既是起点 也是终点 
		
		head = newnode;
		tail = newnode;
		return;//注意返回 不参与下面的操作 
		
	}
	
	newnode->prev = tail;//这两部是把节点插在尾部 
	tail->next = newnode;
	
	tail = tail->next;//更新尾部 
	
}

int main(){
	
	int x;
	Node* head = NULL;
	Node* tail = NULL;
	
	while(cin >> x){
		insert(head, tail, x);
		
		if(cin.peek() == '\n') break;
		
	}
	
	fprintbegin(head);
	cout << endl;
	
	fprintend(tail);
	cout << endl;
	
}
```
---
---
## 输入若干个整数，以回车作为结束符，用链表存储这些数后将链表翻转，输出翻转后的链表
```c
//翻转函数 
void reverse(Node*& head){//head要改变 所以是*& 
	
	Node* h = new Node();//创建新的头节点 
	h->data = 0;
	h->next = NULL;
	
	Node* p = head;
	
	while(p != NULL){//p != null 
		
		Node* q = p->next;//q保存后面的节点 
		
		p->next = h->next;//头插法 
		h->next = p;
		
		p = q;// p 向后移动，继续处理原链表剩余节点
		
	}
	
	head = h->next;//翻转完成 新的head是h的下一个节点 
	
}
```
---
---
## 求两个链表的公共节点.
	步骤：
	求两个链表长度
	长链表先走差值步
	同时移动直到相遇
---
```c
Node* fsame(Node* p, Node* q){

    while(p != q){
        p = p->next;
        q = q->next;
    }

    return p;
}
```
---
---
## 约瑟夫环问题，输入整数n和m，即有n个人从1开始编号围成一个环，从第一个人开始报数（从1开始报），报到m的人走出环外，然后接着由下一个人从1开始报数，求最后一个走出环外的人编号是多少，例如输入4，2，输出1
__构成环就行 不是双向循环链表__
__每次删除节点时 先让节点指向null 然后再delete__


---
	先构成环 
	然后while循环模拟不停报数  while结束条件是只剩下一个人
	while里用for循环模拟报数 每次for循环 踢出去一个人
```c
#include <iostream>
using namespace std;

struct Node{
	
	int data;
	Node* next;
	
};

void insert(Node*& head, Node*& tail, int x){
	
	Node* newnode = new Node();
	newnode->data = x;
	newnode->next = NULL;
	
	if(head == NULL){
		head = newnode;
		tail = newnode;//引入Tail的目的是形成环 而且insert函数里不需要p遍历了 直接让tail指向新节点就行 
		return;
	}
	
	//把新节点插在尾部 
	tail->next = newnode;
	tail = newnode;
	
}

int main(){
	
	int n, m;
	cin >> n >> m;
	Node* head = NULL;
	Node* tail = NULL;
	
	for(int i = 1; i <= n; i++){
		insert(head, tail, i);
	}
	
	tail->next = head;//形成环
	
	Node* p = head;
	Node* prev = tail;//prev指向p的前一个节点 用于每轮报数时删除p 和下一轮从哪里开始的定位 
	//这里给让prev指向tail  是因为如果报的数是1 直接输出的就是5 下面while里的for不执行  只执行剔除p  让prev指向tail才能正常进行 
	
	while(p->next != p){//while的结束条件：只剩下一个人 即p->next = p 
		
		for(int i = 1; i < m; i++){//i小于m就行 因为这时p已经移动到报m的人的位置 
			
			prev = p;
			p = p->next;
			
		}
		
		//一轮报数结束 踢出去p
		prev->next = p->next;
		p->next = NULL;
		delete p;
		
		p = prev->next; //p是下一轮报数的开始位置 是prev->next 
		
	}
	
	cout << p->data << endl;
	 
}



```
---
---
## 给定一个链表,判断是否有环,有返回入环的第一个节点,无返回空
	第一步：判断有没有环

	设置两个指针：
	slow 每次走1步
	fast 每次走2步

	如果有环：
	fast 和 slow 一定会相遇

	如果没有环：
	fast 或 fast->next 会变成 NULL


	第二步：找到入环节点

	当 fast == slow 时：
	再设一个指针 p 从 head 出发。

	然后：
	p 每次走1步
	slow 每次走1步

	两者再次相遇的位置：就是入环节点

	这是一个很经典的数学结论。
```c
#include <iostream>
using namespace std;

struct Node{
    int data;
    Node* next;
};

Node* detectCycle(Node* head){

    Node* slow = head;
    Node* fast = head;

    // 第一阶段：判断是否有环
    while(fast != NULL && fast->next != NULL){

        slow = slow->next;
        fast = fast->next->next;

        if(slow == fast){ // 相遇说明有环

            Node* p = head;

            // 第二阶段：寻找入环节点
            while(p != slow){
                p = p->next;
                slow = slow->next;
            }

            return p; // 入环节点
        }
    }

    return NULL; // 无环
}
```
---
---
# 打印
---
## 输入一个字母和一个数字。 例如 t 3  输出如下 是一个空心菱形

```c
    t
   u a
  v   z
   w y
    x
```
---
__左偏移量和右偏移量 恒为定值 为所有输出字母的总个数__

---
```c
#include <iostream>
#include<cmath>//cmath  不是math.h
using namespace std;
 
void fspace(int n){
	for(int i = 0; i < n; i++){
		cout << " ";
	}
}

//x代表一个字符的ASCII 但可能超过'z'  这个函数就是控制它在a-z之间 然后输出 
void fletter(int x){
	printf("%c",(x - 'a')%26 + 'a');
}

int main(){
	int x;
	char c;
	cin >> c >> x;
	int L = 4 * (x - 1);//L代表总共的字母个数 
	int row = 2 * x - 1;//行数 
	
	for(int i = 1; i <= row; i++){
		int index = x - abs(x - i);//12321    index可以用来确定字母前后的空格数  index = x - abs(x-i)
		int offset = i - 1;//offset表示相对于输入的字母的偏移量 
		
		fspace(x - index);//输出空格 
		fletter(c + offset);//左侧字母 偏移量是offset  加上开始的字母c  c+offset 就代表现在的ASCII 
		
		if(i != 1 && i != row){//不是第一行和最后一行  要输出中间的空格和右侧的字母 
			fspace(2*index - 3);
			fletter(c + L - offset);//右侧字母  总共字母数是 L 左边的字母偏移了 offset  右侧偏移 L-offset 
		}
		cout << endl;
		
	}
	
}
```
---

