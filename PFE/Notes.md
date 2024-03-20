Notice how he says 
In order to better understand the architectures we’re choosing from when we make this decision, we need to have a solid understanding of each approach and consistent terminology to use when speaking about them. The differences between these approaches help illustrate the trade-offs of rendering on the web through the lens of performance.
https://web.dev/rendering-on-the-web/
![[Pasted image 20221114164610.png]]
```
how to simulate multiple users sending a request to my local server ?

There are a few different ways you can simulate multiple users sending requests to your local server:

1.  Use a load testing tool: There are a number of load testing tools available that can help you simulate multiple users sending requests to your server. These tools can create a specified number of virtual users (also known as "threads") that send requests to your server concurrently. Some popular load testing tools include `Apache JMeter`, `LoadRunner`, and `Gatling`.
    
2.  Use a script: You can also write a script that sends multiple requests to your server concurrently using a programming language such as Python or Ruby. For example, you could use the `multiprocessing` module in Python to create multiple processes that send requests to your server simultaneously.
    
3.  Use a browser extension: Some browser extensions, such as `Apache JMeter's HTTP(S) Test Script Recorder`, can help you simulate multiple users sending requests to your server. These extensions allow you to record your interactions with a website and replay them multiple times concurrently.
    

By using one of these methods, you should be able to simulate multiple users sending requests to your local server and test the performance and scalability of your server. It's important to note that the performance of your server may vary depending on the specific hardware and software configurations of your system.
```

```
how to benchmark different caching srategies ?

To benchmark different caching strategies, you will need to follow these steps:

1.  Define the caching strategies you want to compare. You will need to implement each caching strategy as a separate function or class. Make sure to include any necessary data structures or variables that each strategy will use to store and retrieve cached data.
    
2.  Define a test dataset that you will use to evaluate the performance of each caching strategy. This dataset should be representative of the types of data you expect to cache in your application.
    
3.  Write a benchmarking function that will test the performance of each caching strategy. This function should take the test dataset as input and measure the time taken to execute each caching strategy on the dataset. You can use Rust's built-in benchmarking library, `criterion`, to help with this.
    
4.  Run the benchmarking function and analyze the results. You should compare the performance of each caching strategy using a variety of metrics, such as the average time taken to execute the strategy, the variance in the execution time, and the number of cache hits and misses.
    

By following these steps, you should be able to benchmark different caching strategies and compare their performance. It's important to note that the performance of a caching strategy can depend on a variety of factors, such as the size of the dataset, the frequency of cache hits and misses, and the complexity of the caching algorithm. As a result, you may need to run the benchmarking function multiple times with different datasets and configurations to get a comprehensive understanding of the performance of each caching strategy.
```

