@2024.06.13 | Week 16

### 1. Two thread program

观察程序，认为输出应为一个介于 `[1,000,000, 2,000,000]` 间的值。

编写脚本，运行 `main.exe` 100 次，得到程序输出的平均值：`1151032.16`。

![[Snipaste_240614_192550.png]]

> [!question] What is the problem in the program?

本程序存在竞态条件：两个线程竞争地读取、修改同一个全局变量。

> [!question] Give me the term that describe this phenomenon

竞态条件(race condition)

> [!question] Why this happen?

因为两个线程执行的先后顺序是未知的，下图展示了一种可能的出错情况：

![[slp lab - race condition.png|300]]

### 2. Add thread to database searching

```cpp
void retrievePersonalInfo(int account, Personal **pers)
{
	*pers = GetPersonalInformation(account);
}

void retrieveAccountInfo(int account, AccountInfo **acct)
{
	*acct = GetAccountInformation(account);
}

int main(int argc, char *argv[])
{
	if (argc != 2)
	{
		cerr << "usage: " << argv[0] << " [account_number]" << endl;
		exit(1);
	}
	int account = atoi(argv[1]);
	
	time_ms_t start = getTimeInMilliseconds();
	cout << "Retrieving...";
	cout.flush();
	
	Personal *pers = nullptr;
	AccountInfo *acct = nullptr;
	
	std::thread t1(retrievePersonlaInfo, account, &pers);
	std::thread t2(retrieveAccountInfo, account, &acct);
	
	t1.join();
	t2.join();
	
	time_ms_t end = getTimeInMilliseconds();
	cout << "done (" << end - start << "ms)" << endl;
	
	if (pers)
	{
		cout << account << ": " << pers->FirstName << " "
			 << pers->LastName << endl;
		cout << pers->Address << endl;
		cout << "Balance: " << acct->Balance << ", " << acct->Pending
			 << " pending, " << acct->Share << " share" << endl;
		
		delete pers;
		delete acct;
	}
	else
		cout << "No client matches that account number" << endl;
	return 0;
}
```