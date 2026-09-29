# Operating Systems Practical

Simple C++ programs for OS practicals and viva preparation.

## Programs

* FCFS Scheduling
* SJF Scheduling
* Round Robin Scheduling
* Thread Creation
* Fork / Child Process
* Parent-Child Process with PID
* Fork with Wait

---

## FCFS

```cpp
#include <iostream>
using namespace std;

int main()
{
    int n;
    cout << "Enter number of processes: ";
    cin >> n;

    int bt[20], wt[20], tat[20];

    for (int i = 0; i < n; i++)
    {
        cout << "Enter burst time of P" << i + 1 << ": ";
        cin >> bt[i];
    }

    wt[0] = 0;

    for (int i = 1; i < n; i++)
        wt[i] = wt[i - 1] + bt[i - 1];

    for (int i = 0; i < n; i++)
        tat[i] = wt[i] + bt[i];

    cout << "\nProcess\tBT\tWT\tTAT\n";

    for (int i = 0; i < n; i++)
    {
        cout << "P" << i + 1 << "\t"
             << bt[i] << "\t"
             << wt[i] << "\t"
             << tat[i] << endl;
    }

    return 0;
}
```

---

## SJF

```cpp
#include <iostream>
using namespace std;

int main()
{
    int n;
    cout << "Enter number of processes: ";
    cin >> n;

    int bt[20], wt[20], tat[20], p[20];

    for (int i = 0; i < n; i++)
    {
        p[i] = i + 1;

        cout << "Enter burst time of P" << i + 1 << ": ";
        cin >> bt[i];
    }

    for (int i = 0; i < n - 1; i++)
    {
        for (int j = i + 1; j < n; j++)
        {
            if (bt[i] > bt[j])
            {
                int temp = bt[i];
                bt[i] = bt[j];
                bt[j] = temp;

                temp = p[i];
                p[i] = p[j];
                p[j] = temp;
            }
        }
    }

    wt[0] = 0;

    for (int i = 1; i < n; i++)
        wt[i] = wt[i - 1] + bt[i - 1];

    for (int i = 0; i < n; i++)
        tat[i] = wt[i] + bt[i];

    cout << "\nProcess\tBT\tWT\tTAT\n";

    for (int i = 0; i < n; i++)
    {
        cout << "P" << p[i] << "\t"
             << bt[i] << "\t"
             << wt[i] << "\t"
             << tat[i] << endl;
    }

    return 0;
}
```

---

## Round Robin

```cpp
#include <iostream>
using namespace std;

int main()
{
    int n, quantum;

    cout << "Enter number of processes: ";
    cin >> n;

    int bt[20], rem[20];

    for (int i = 0; i < n; i++)
    {
        cout << "Enter burst time of P" << i + 1 << ": ";
        cin >> bt[i];

        rem[i] = bt[i];
    }

    cout << "Enter time quantum: ";
    cin >> quantum;

    int time = 0;

    while (true)
    {
        bool done = true;

        for (int i = 0; i < n; i++)
        {
            if (rem[i] > 0)
            {
                done = false;

                if (rem[i] > quantum)
                {
                    time += quantum;
                    rem[i] -= quantum;
                }
                else
                {
                    time += rem[i];
                    rem[i] = 0;

                    cout << "P" << i + 1
                         << " completed at time "
                         << time << endl;
                }
            }
        }

        if (done)
            break;
    }

    return 0;
}
```

---

## Thread

```cpp
#include <iostream>
#include <pthread.h>

using namespace std;

void* run(void* arg)
{
    cout << "Thread is running" << endl;

    return NULL;
}

int main()
{
    pthread_t t;

    pthread_create(&t, NULL, run, NULL);

    pthread_join(t, NULL);

    cout << "Main function finished" << endl;

    return 0;
}
```

Compile:

```bash
g++ thread.cpp -o thread -pthread
```

Run:

```bash
./thread
```

---

## Fork

```cpp
#include <iostream>
#include <unistd.h>

using namespace std;

int main()
{
    int pid;

    pid = fork();

    if (pid == 0)
    {
        cout << "I am the child process" << endl;
    }
    else if (pid > 0)
    {
        cout << "I am the parent process" << endl;
    }
    else
    {
        cout << "Fork failed" << endl;
    }

    return 0;
}
```

---

## Parent Child PID

```cpp
#include <iostream>
#include <unistd.h>

using namespace std;

int main()
{
    int pid;

    pid = fork();

    if (pid == 0)
    {
        cout << "Child process" << endl;
        cout << "My PID: " << getpid() << endl;
        cout << "My Parent PID: " << getppid() << endl;
    }
    else if (pid > 0)
    {
        cout << "Parent process" << endl;
        cout << "My PID: " << getpid() << endl;
        cout << "Child PID: " << pid << endl;
    }
    else
    {
        cout << "Fork failed" << endl;
    }

    return 0;
}
```

---

## Fork + Wait

```cpp
#include <iostream>
#include <unistd.h>
#include <sys/wait.h>

using namespace std;

int main()
{
    int pid = fork();

    if (pid == 0)
    {
        cout << "Child is running" << endl;
    }
    else if (pid > 0)
    {
        wait(NULL);

        cout << "Parent is running" << endl;
    }

    return 0;
}
```

---

## Quick Viva

| Topic              | Crux                            |
| ------------------ | ------------------------------- |
| FCFS               | First come, first served        |
| SJF                | Shortest burst time first       |
| Round Robin        | Fixed time quantum              |
| Thread             | Execution unit inside a process |
| `pthread_create()` | Creates a thread                |
| `pthread_join()`   | Waits for a thread              |
| `fork()`           | Creates a child process         |
| `fork() == 0`      | Child                           |
| `fork() > 0`       | Parent                          |
| `fork() < 0`       | Error                           |
| `getpid()`         | Current process ID              |
| `getppid()`        | Parent process ID               |
| `wait()`           | Parent waits for child          |
| WT                 | `TAT - BT`                      |
| TAT                | `CT - AT`                       |

## Compilation

```bash
g++ fcfs.cpp -o fcfs
g++ sjf.cpp -o sjf
g++ rr.cpp -o rr
g++ thread.cpp -o thread -pthread
g++ fork.cpp -o fork
g++ parent_child.cpp -o parent_child
g++ fork_wait.cpp -o fork_wait
```

Run any program with:

```bash
./program_name
```
