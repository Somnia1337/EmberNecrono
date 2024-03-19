#### 并查集（按秩合并）

```java
import java.util.*;

public class WeightedQuickUnionUF {
    private int[] root, size;
    private int count;
	
    public WeightedQuickUnionUF(int N) {
        count = N;
        root = new int[N];
        Arrays.setAll(root, a -> a);
        size = new int[N];
        Arrays.fill(size, 1);
    }
	
    public int count() {
        return count;
    }
	
    public boolean connected(int a, int b) {
        return find(a) == find(b);
    }
	
    public int find(int x) {
        while (x != root[x]) x = root[x];
        return x;
    }
	
    public void union(int a, int b) {
        int r1 = find(a), r2 = find(b);
        if (r1 == r2) return;
        if (size[r1] < size[r2]) {
            root[r1] = r2;
            size[r2] += size[r1];
        }
        else {
            root[r2] = r1;
            size[r1] += size[r2];
        }
        count--;
    }
}
```

#### guessing_game

```rust
use rand::Rng;
use std::cmp::Ordering;
use std::io;

fn main() {
    println!("Guess the number!");
	
    let secret_number = rand::thread_rng().gen_range(1..=100);
	
    loop {
        println!("Please input your guess.");
		
        let mut guess = String::new();
		
        io::stdin()
            .read_line(&mut guess)
            .expect("Failed to read line");
			
        let guess: u32 = match guess.trim().parse() {
            Ok(num) => num,
            Err(_) => continue,
        };
		
        println!("You guessed: {guess}");
		
        match guess.cmp(&secret_number) {
            Ordering::Less => println!("Too small!"),
            Ordering::Greater => println!("Too big!"),
            Ordering::Equal => {
                println!("You win!");
                break;
            }
        }
    }
}
```

#### minigrep

```rust
// main.rs
use std::{env, process};
use minigrep::{Config, run};

fn main() {
    let config = Config::build(env::args()).unwrap_or_else(|err| {
        eprintln!("Problems parsing arguments: {err}");
        process::exit(1);
    });
    if let Err(e) = run(config) {
        eprintln!("Application error: {e}");
        process::exit(1);
    }
}
```

```rust
// lib.rs
use std::{env, error::Error, fs};

pub struct Config {
    pub query: String,
    pub file_path: String,
    pub ignore_case: bool,
}

impl Config {
    pub fn build(
        mut args: impl Iterator<Item=String>,
    ) -> Result<Config, &'static str> {
        args.next();
        let query = match args.next() {
            Some(arg) => arg,
            None => return Err("Expected a query string"),
        };
        let file_path = match args.next() {
            Some(arg) => arg,
            None => return Err("Expected a file path"),
        };
        let ignore_case = env::var("IGNORE_CASE").map_or(false, |var| var.eq("1"));
        Ok(Config {
            query,
            file_path,
            ignore_case,
        })
    }
}

pub fn run(config: Config) -> Result<(), Box<dyn Error>> {
    let contents = fs::read_to_string(config.file_path)?;
    
    let res = if config.ignore_case {
        search_case_insensitive(&config.query, &contents)
    } else {
        search(&config.query, &contents)
    };
    
    for line in res {
        println!("{line}");
    }
    
    Ok(())
}

pub fn search<'a>(query: &str, contents: &'a str) -> Vec<&'a str> {
    contents
        .lines()
        .filter(|line| line.contains(query))
        .collect()
}

pub fn search_case_insensitive<'a>(
    query: &str,
    contents: &'a str,
) -> Vec<&'a str> {
    let query = query.to_lowercase();
    let mut res = vec![];
    for line in contents.lines() {
        if line.to_lowercase().contains(&query) {
            res.push(line);
        }
    }
    res
}

#[cfg(test)]
mod tests {
    use super::*;
    
    #[test]
    fn one_result() {
        let query = "duct";
        let contents = "\
Rust:
safe, fast, productive.
Pick three.";
        
        assert_eq!(vec!["safe, fast, productive."], search(query, contents));
    }
    
    #[test]
    fn case_insensitive() {
        let query = "rUsT";
        let contents = "\
Rust:
safe, fast, productive.
Pick three.
Trust me.";
        
        assert_eq!(
            vec!["Rust:", "Trust me."],
            search_case_insensitive(query, contents)
        );
    }
}
```

#### simple_web_server

##### v1. 单线程 server

```rust
use std::{
    fs,
    io::{prelude::*, BufReader},
    net::{TcpListener, TcpStream},
};

fn main() {
    let listener = TcpListener::bind("127.0.0.1:7878").unwrap();
    for stream in listener.incoming() {
        let stream = stream.unwrap();
        handle_connection(stream);
    }
}

/// 处理一个 TCP 流
///
/// 解析请求, 选择响应的信息和文件, 并进行响应
fn handle_connection(mut stream: TcpStream) {
    // 读取请求的第 1 行
    let buf_reader = BufReader::new(&mut stream);
    let request_line = buf_reader.lines().next().unwrap().unwrap();
    
    // 选择响应的信息和文件
    let (status_line, filename) =
        if request_line == "GET / HTTP/1.1" {
            ("HTTP/1.1 200 OK", "html\\hello.html")
        } else {
            ("HTTP/1.1 404 NOT FOUND", "html\\404.html")
        };
    
    // 响应
    let contents = fs::read_to_string(filename).unwrap();
    let length = contents.len();
    let response =
        format!("{status_line}\r\nContent-Length: {length}\r\n\r\n{contents}");
    stream.write_all(response.as_bytes()).unwrap();
}
```

##### v2. 多线程 server

```rust
// main.rs
use std::{
    fs,
    io::{prelude::*, BufReader},
    net::{TcpListener, TcpStream},
};
use simple_web_server::ThreadPool;

fn main() {
    let listener = TcpListener::bind("127.0.0.1:7878").unwrap();
    let pool = ThreadPool::new(4);
    for stream in listener.incoming() {
        let stream = stream.unwrap();
        pool.execute(|| {
            handle_connection(stream);
        });
    }
}

/// 处理一个 TCP 流
///
/// 解析请求, 选择响应的信息和文件, 并进行响应
fn handle_connection(mut stream: TcpStream) {
    // 读取请求的第 1 行
    let buf_reader = BufReader::new(&mut stream);
    let request_line = buf_reader.lines().next().unwrap().unwrap();
    
    // 选择响应的信息和文件
    let (status_line, filename) =
        if request_line == "GET / HTTP/1.1" {
            ("HTTP/1.1 200 OK", "html\\hello.html")
        } else {
            ("HTTP/1.1 404 NOT FOUND", "html\\404.html")
        };
    
    // 响应
    let contents = fs::read_to_string(filename).unwrap();
    let length = contents.len();
    let response =
        format!("{status_line}\r\nContent-Length: {length}\r\n\r\n{contents}");
    stream.write_all(response.as_bytes()).unwrap();
}
```

```rust
// lib.rs
use std::{thread, sync::{mpsc, Arc, Mutex}};
use std::sync::mpsc::Receiver;

struct Worker {
    id: usize,
    thread: thread::JoinHandle<()>,
}

impl Worker {
    fn new(id: usize, receiver: Arc<Mutex<Receiver<Job>>>) -> Worker {
        let thread = thread::spawn(move || loop {
            let job = receiver.lock().unwrap().recv().unwrap();
            println!("Worker {id} got a job; executing...");
            job();
        });
        Worker { id, thread }
    }
}

// 类型别名, 将长类型名变短
type Job = Box<dyn FnOnce() + Send + 'static>;

pub struct ThreadPool {
    workers: Vec<Worker>,
    sender: mpsc::Sender<Job>,
}

impl ThreadPool {
    /// 创建线程池
    ///
    /// 线程池中线程的数量
    ///
    /// # Panics
    ///
    /// 在 `size` 为 `0` 时 panic
    pub fn new(size: usize) -> ThreadPool {
        assert!(size > 0);
        let (sender, receiver) = mpsc::channel();
        let receiver = Arc::new(Mutex::new(receiver));
        let mut workers = Vec::with_capacity(size);
        for id in 0..size {
            workers.push(Worker::new(id, Arc::clone(&receiver)));
        }
        ThreadPool { workers, sender }
    }
    
    pub fn execute<F>(&self, f: F)
        where
            F: FnOnce() + Send + 'static,
    {
        let job = Box::new(f);
        self.sender.send(job).unwrap()
    }
}
```

##### v3. 优雅停机

```rust
// main.rs
use std::{
    fs,
    io::{prelude::*, BufReader},
    net::{TcpListener, TcpStream},
};
use simple_web_server::ThreadPool;

fn main() {
    let listener = TcpListener::bind("127.0.0.1:7878").unwrap();
    let pool = ThreadPool::new(4);
    for stream in listener.incoming().take(2) {
        let stream = stream.unwrap();
        pool.execute(|| {
            handle_connection(stream);
        });
    }
    println!("Server shutting down...");
}

/// 处理一个 TCP 流
///
/// 解析请求, 选择响应的信息和文件, 并进行响应
fn handle_connection(mut stream: TcpStream) {
    // 读取请求的第 1 行
    let buf_reader = BufReader::new(&mut stream);
    let request_line = buf_reader.lines().next().unwrap().unwrap();
    
    // 选择响应的信息和文件
    let (status_line, filename) =
        if request_line == "GET / HTTP/1.1" {
            ("HTTP/1.1 200 OK", "html\\hello.html")
        } else {
            ("HTTP/1.1 404 NOT FOUND", "html\\404.html")
        };
    
    // 响应
    let contents = fs::read_to_string(filename).unwrap();
    let length = contents.len();
    let response =
        format!("{status_line}\r\nContent-Length: {length}\r\n\r\n{contents}");
    stream.write_all(response.as_bytes()).unwrap();
}
```

```rust
// lib.rs
use std::{thread, sync::{mpsc, Arc, Mutex}};
use std::sync::mpsc::Receiver;

struct Worker {
    id: usize,
    thread: Option<thread::JoinHandle<()>>,
}

impl Worker {
    fn new(id: usize, receiver: Arc<Mutex<Receiver<Job>>>) -> Worker {
        let thread = thread::spawn(move || loop {
            let msg = receiver.lock().unwrap().recv();
            match msg {
                Ok(job) => {
                    println!("Worker {id} got a job; executing...");
                    job();
                }
                Err(_) => {
                    println!("Worker {id} disconnected; shutting down...");
                    break;
                }
            }
        });
        Worker { id, thread: Some(thread) }
    }
}

// 类型别名, 将长类型名变短
type Job = Box<dyn FnOnce() + Send + 'static>;

pub struct ThreadPool {
    workers: Vec<Worker>,
    sender: Option<mpsc::Sender<Job>>,
}

impl ThreadPool {
    /// 创建线程池
    ///
    /// 线程池中线程的数量
    ///
    /// # Panics
    ///
    /// 在 `size` 为 `0` 时 panic
    pub fn new(size: usize) -> ThreadPool {
        assert!(size > 0);
        let (sender, receiver) = mpsc::channel();
        let receiver = Arc::new(Mutex::new(receiver));
        let mut workers = Vec::with_capacity(size);
        for id in 0..size {
            workers.push(Worker::new(id, Arc::clone(&receiver)));
        }
        ThreadPool { workers, sender: Some(sender) }
    }
    
    pub fn execute<F>(&self, f: F)
        where
            F: FnOnce() + Send + 'static,
    {
        let job = Box::new(f);
        self.sender.as_ref().unwrap().send(job).unwrap()
    }
}

impl Drop for ThreadPool {
    fn drop(&mut self) {
        drop(self.sender.take());
        for worker in &mut self.workers {
            println!("Shutting down worker {}", worker.id);
            if let Some(thread) = worker.thread.take() {
                thread.join().unwrap();
            }
        }
    }
}
```

#### 面向对象

##### v1. 完全的状态模式

```rust
// main.rs
use rs_master::Post;

fn main() {
    let mut post = Post::new();
    
    post.add_text("I ate a salad for lunch today");
    assert_eq!("", post.content());
    
    post.request_review();
    assert_eq!("", post.content());
    
    post.approve();
    assert_eq!("I ate a salad for lunch today", post.content());
}
```

```rust
// lib.rs
pub struct Post {
    state: Option<Box<dyn State>>,
    content: String,
}

impl Post {
    pub fn new() -> Post {
        Post {
            state: Some(Box::new(Draft {})),
            content: String::new(),
        }
    }
    pub fn add_text(&mut self, text: &str) {
        self.content.push_str(text);
    }
    
    pub fn content(&self) -> &str {
        self.state.as_ref().unwrap().content(self)
    }
    
    pub fn request_review(&mut self) {
        if let Some(s) = self.state.take() {
            self.state = Some(s.request_review())
        }
    }
    
    pub fn approve(&mut self) {
        if let Some(s) = self.state.take() {
            self.state = Some(s.approve())
        }
    }
}

trait State {
    fn request_review(self: Box<Self>) -> Box<dyn State>;
    fn approve(self: Box<Self>) -> Box<dyn State>;
    fn content<'a>(&self, post: &'a Post) -> &'a str {
        ""
    }
}

struct Draft {}

impl State for Draft {
    fn request_review(self: Box<Self>) -> Box<dyn State> {
        Box::new(PendingReview {})
    }
    
    fn approve(self: Box<Self>) -> Box<dyn State> {
        self
    }
}

struct PendingReview {}

impl State for PendingReview {
    fn request_review(self: Box<Self>) -> Box<dyn State> {
        self
    }
    
    fn approve(self: Box<Self>) -> Box<dyn State> {
        Box::new(Published {})
    }
}

struct Published {}

impl State for Published {
    fn request_review(self: Box<Self>) -> Box<dyn State> {
        self
    }
    
    fn approve(self: Box<Self>) -> Box<dyn State> {
        self
    }
    
    fn content<'a>(&self, post: &'a Post) -> &'a str {
        &post.content
    }
}
```

##### v2. 更 Rusty 的设计

```rust
// main.rs
use rs_master::Post;

fn main() {
    let mut post = Post::new();
    post.add_text("I ate a salad for lunch today");
    let post = post.request_review();
    let post = post.approve();
    assert_eq!("I ate a salad for lunch today", post.content());
}
```

```rust
// lib.rs
pub struct Post {
    content: String,
}

impl Post {
    pub fn new() -> Draft {
        Draft {
            content: String::new(),
        }
    }
    
    pub fn content(&self) -> &str {
        &self.content
    }
    
    pub fn add_text(&mut self, text: &str) {
        self.content.push_str(text);
    }
}

pub struct Draft {
    content: String,
}

impl Draft {
    pub fn add_text(&mut self, text: &str) {
        self.content.push_str(text);
    }
    
    pub fn request_review(self) -> PendingReview {
        PendingReview {
            content: self.content,
        }
    }
}

pub struct PendingReview {
    content: String,
}

impl PendingReview {
    pub fn approve(self) -> Post {
        Post {
            content: self.content,
        }
    }
}
```

#### Rust vs Python

```rust
use std::cmp::Ordering;
use rand::Rng;
use std::time::Instant;

fn main() {
    let st = Instant::now();
    let mut total = 0;
    for _ in 0..=1000000 {
        let x = rand::thread_rng().gen_range(1..=100);
        let (mut l, mut r, mut cnt) = (1, 100, 0);
        loop {
            let m = l + r >> 1;
            cnt += 1;
            match m.cmp(&x) {
                Ordering::Less => l = m + 1,
                Ordering::Greater => r = m - 1,
                Ordering::Equal => break,
            }
        }
        total += cnt;
    }
    let ed = Instant::now();
    println!("{}", total as f32 / 10000.0);
    println!("{:?}", ed - st);
}
```

```python
import random
import time

start_time = time.time()

total = 0
for _ in range(1000001):
    x = random.randint(1, 100)
    l, r, cnt = 1, 100, 0
    while True:
        m = (l + r) // 2
        cnt += 1
        if m < x:
            l = m + 1
        elif m > x:
            r = m - 1
        else:
            break
    total += cnt

end_time = time.time()

print(total / 1000000)
print(end_time - start_time)
```