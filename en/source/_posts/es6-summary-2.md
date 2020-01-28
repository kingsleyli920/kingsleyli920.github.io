---
title: es6_summary_2
date: 2020-01-28 00:47:18
tags:
 - JavaScript
 - ECMAScript 6
 - Front End Development
 - Web Development
comments: true
categories: Front-end
img: https://blog-imgs-1253907084.cos.ap-beijing.myqcloud.com/blog-cover-imgs/frontend-js-article1.jpeg
---
<center> <h1>ES6 Summary #2</h1> </center>
<center> <h2> <font color = lightgray>Asynchronous and synchronous</font> </h2> </center>

### ***<u>Asynchronous and synchronous are a message notification mechanism.</u>***

## What is Asynchronous？

### Asynchronous non-blocking：

    For example: A calls B, while B processes, A continues to execute. The processing of B ends, the result is returned, and A is executed again with the result of B. A does not have to wait for the end of B's execution during this period.

### Synchronous blocking：

	For example: A calls B, and B returns the result to A after processing, and A continues to execute. A waits for the end of B's execution during this period. The execution of normal code is performed line by line, which is the synchronization mechanism.
	
### The most basic way to achieve asynchronous: ***Using timers***

	function move(ele, dir, dist) {
        let curPos = parseFloat(getComputedStyle(ele)[dir]);
        let speed = (dist - curPos) /Math.abs(curPos - dist);
        clearInterval(ele.timer);
        console.log("start moving...");
        ele.timer = setInterval(() => {
           if (Math.abs(curPos - dist) <= 0) {
                clearInterval(ele.timer);
                console.log("finish moving...");
           } else {
                curPos += speed;
                ele.style[dir] = curPos + "px";
           }
        }, 20);
        console.log("printing....");
    }

    let box = document.querySelector("#box");

    move(box, "left", 100);
    
The result is：

![Code Result](https://blog-imgs-1253907084.cos.ap-beijing.myqcloud.com/code-result1.gif)

The printing result is：

![Code Printing Result](https://blog-imgs-1253907084.cos.ap-beijing.myqcloud.com/code-result2.png)

It can be seen from the results that "printing..." is printed before the end of the block movement, and it is not executed sequentially as in a regular program. This is the most basic asynchronous implementation.

### Need more operation after asynchronous funtion end?

There must be many solutions, but I think the better method is the callback function method, modify the above code：

	function move(ele, dir, dist, callback) {
        let curPos = parseFloat(getComputedStyle(ele)[dir]);
        let speed = (dist - curPos) /Math.abs(curPos - dist);
        clearInterval(ele.timer);
        console.log("start moving...");
        ele.timer = setInterval(() => {
           if (Math.abs(curPos - dist) <= 0) {
                clearInterval(ele.timer);
                console.log("finish moving...");
                callback && callback(); // ===>First determine whether there is a callback, if it exists, then execute
           } else {
                curPos += speed;
                ele.style[dir] = curPos + "px";
           }
        }, 20);
        console.log("printing....");

    }

    let box = document.querySelector("#box");

    move(box, "left", 100, () => {
        console.log("Run callback function");
    });
 
The result is：
 
	start moving...
	printing....
	finish moving...
	Run callback function
	
This can well solve the problems that require asynchronous results or need to execute some code after asynchronous execution.

### This implementation of asynchronous and callback issues: ***Callback Hell***
The Callback Hell is to call call back function multiple times, resulting in too many layers, as follows:
	
	move(box, "left", 100,() => {
        move(box, "top", 100,() => {
            move(box, "left", -100,() => {
                move(box, "top", -100,() => {
                    console.log("finish...");
                });
            });
        });
    });
    
The layer of above code is too deep, which is not conducive to code reuse, maintainability, and readability.

## Callback Hell Solution

There are three solutions, namely using **Promise**, using **Async and Await**, and using **Generator** function. I will talk about the Promise.

### Promise concept and usage

***Promises do not solve the asynchronous problem itself, but solve the asynchronous writing problem, making asynchronous writing clearly.***

Let's talk about promises first. In MDN, promises are defined as follows:
	
	The Promise object represents the eventual completion (or failure) of an asynchronous operation, and its resulting value.[1]
	
Promise has three states: <b> Pending (waiting for asynchronous process execution to complete); Fulfilled or Resolved (successful); Rejected (failed) </ b>.
The result of the asynchronous operation determines the current state, and other content cannot interfere with the Promise state. In addition, if the status of a Promise changes, it will not change again. For example, if you change from a Pending turn to Rejected, state will not return to Pending or become Fulfilled again. [2]

The following shows a simple use of a Promise:

	let p = new Promise((resolve, reject) => {
        	setTimeout(() => {
	            console.log(p);
	            resolve();
        	}, 2000); 
    	});

    	p.then(() => {
        	console.log(p);
        	console.log("then....");
    	}); 
    	
The "then" method is equivalent to the callback function. According to the above code, it can be found that a function is passed after the Promise is created. The parameters of the function are two functions - Resolve and Reject, corresponding to Rejected and Resolved (Fulfilled) States. Now you can see at a glance. If the asynchronous execution is successful, you call "resolve()", otherwise "reject()". The above results are:

After the program runs for two seconds, it prints the following：
	
	Promise {<pending>}
	Promise {<resolved>: undefined}
	then....
	
It can be seen that if the asynchronous execution is successful and the Resolve function is called, the program will execute down to "then", and from the two printed "p"s, we can see that the status of the Promise changes from Pending to Resolved.

#### So how do you use asynchronous results?

Many times, the results produced by asynchronous are what we will use later, so if we use Promise to optimize asynchronous writing, like above, how do we use asynchronous results?

In fact, the answer already appears in the above code. The second line of the above code results, There is an "undefined" value after "resolved". This place is used to store asynchronous results. The code can be changed as follows:

	let p = new Promise((resolve, reject) => {
	        setTimeout(() => {
	            console.log(p);
	            resolve("parameters..."); // =======》Here you can pass parameters when executing resolve.
	        }, 2000); 
	 });
	
	 p.then((res) => { // ======》You can pass a parameter here, such asres
	        console.log(p);
	        console.log(res); 		
	        console.log("then....");
	 });

The results are：

	Promise {<pending>}
	Promise {<resolved>: "parameters..."}
	parameters...
	then....

Combining the above code and results, we can see that by passing the asynchronous result to the resolve method, after calling the then method, it can receive a parameter, which will point to the asynchronous result. This makes it possible to use asynchronous results.

If you want to use the result of "reject" in the "then" function, it is also very simple. Pass a second callback function as a parameter, such as:
	
	p.then((res) => {
	        console.log(p);
	        console.log(res);
	        console.log("then....");
	 }, (rej) => {
	        console.log(rej);
	 });
	 
At a glance, I won't go into details here.

However, if we should call the "then" function for multiple times, two functions are passed in each function to handle the success and failure results, which is sometimes cumbersome. Therefore, if all steps are processed with errors or failures, but only one kind of information need to be given to the user (such as network request error), you can use the following methods:

	p.then((res) => {
	        console.log(p);
	        console.log(res);
	        console.log("then....");
	 }).then((res) => {
	        console.log(p);
	        console.log(res);
	        console.log("then....");
	 }).then((res) => {
	        console.log(p);
	        console.log(res);
	        console.log("then....");
	 }).catch((rej) => {
	 	console.log("fail...");
	 })


If you want to implement the previous scenario that requires multiple callbacks, the code is as follows:

	let po = new Promise((resolve, reject) => {
        resolve(1);
    });

    po.then((res) => {
       console.log(res);
       return 2;
    }).then((res) => {
       console.log(res);
       return 3;
    }).then((res) => {
       console.log(res);
       return 4;
    }).then((res) => {
       console.log(res);
    })
    
Result is：
 
	1
	2
	3
	4
	
Obviously, the result returned by the "then" method can be obtained by the "then" method behind. But why is it available? We can assign a value to the result of the "then" method and print it. The code is:

	let po = new Promise((resolve, reject) => {
         resolve(1);
     });

     let pt = po.then((res) => {
         console.log(res);
         return "pt";
     })

     console.log(pt);
     
 Result is：
 
	Promise {<pending>}__proto__: Promise[[PromiseStatus]]: "resolved"[[PromiseValue]]: "pt"
	1

After the above code is run, the last line of "pt" is printed. Because of the asynchronous reason, then the number "1" of the resolve passed into the "then" function is printed. The expansion result of "pt" can be seen that the "then" method will return a Promise object to us, and from the above results, it is seen that a Promise object with "resolved" state is returned.

Now that we all understand the use of promises, the example of moving blocks above can be rewritten as:

	function move(ele, dir, dist) {
        let curPos = parseFloat(getComputedStyle(ele)[dir]);
        let speed = (dist - curPos) /Math.abs(curPos - dist);

        return new Promise((resolve, reject) => {
            clearInterval(ele.timer);
            ele.timer = setInterval(() => {
            if (Math.abs(curPos - dist) <= 0) {
                    clearInterval(ele.timer); 
                    resolve();
            } else {
                    curPos += speed;
                    ele.style[dir] = curPos + "px";
            }
            }, 20);
        })
    }

    let box = document.querySelector("#box");

    move(box, "left", 100).then(() => {
        return move(box, "top", 100);
    }).then(() => {
        return move(box, "left", 0);
    }).then(() => {
        return move(box, "top", 0);
    })
    
The result is the same as the above result. But readability and code scalability have been significantly improved.

#### Promise "all" function

After talking about the use of Promise and the use of "catch" and "then", later, we will talk about another common method, Promise "all" function.

MDN's description of the "all" method is:
	
	The Promise.all() method returns a single Promise that fulfills when all of the promises passed as an iterable have been fulfilled or when the iterable contains no promises. It rejects with the reason of the first promise that rejects.[3] 
	
For example, three different Promise objects are created, and the "all" method is called to execute the three Promise objects. If all three Promise objects are in the "resolved" state, then, can be executed downwards.

For example:
	
	 let p1 = new Promise((resolve, reject) => {
         console.log(1);
         resolve();
     });


    let p2 = new Promise((resolve, reject) => {
         console.log(2);
         resolve();
     });

     let p3 = new Promise((resolve, reject) => {
         console.log(3);
         resolve();
     });

     Promise.all([p1, p2, p3]).then(() =>{
         console.log(4);
     })
	
Result:

	1
	2
	3
	4
	
If the execution status of any Promise object is rejected. You need to use catch method to catch errors in "then" or at the end of the code, otherwise an error will be reported.

### Promise "race" function

This method is exactly the opposite of the all method. The all method is executed. And after all execution is complete, the following content is executed. As the name of the race method implies, when one of the executions of Promise objects ends, the following method starts to execute. code show as below:

	 let p1 = new Promise((resolve, reject) => {
         setTimeout(() => {
            console.log(1);
            resolve();
         }, 2000);
     });


    let p2 = new Promise((resolve, reject) => {
        setTimeout(() => {
            console.log(2);
            resolve();
         }, 1000);
     });

     let p3 = new Promise((resolve, reject) => {
        setTimeout(() => {
            console.log(3);
            resolve();
         }, 5000);
     });

     Promise.race([p1, p2, p3]).then(() =>{
         console.log(4);
     }).catch(() => {
         console.log("err");
     });

结果是：
	
	2
	4
	1
	3
	
In the above code, we set the execution time of "p1" to 2s, "p2" to 1s, and "p3" to 5s. As can be seen from the results, after the execution of "p1", the following "then" method is directly executed, so the printing order is 2, 4 and the last two will finish later.

### Async and Await

Sometimes when using the series of Promise methods, I still feel that the readability is general and the code scalability is limited. At this time, you can use Async and Await in combination with Promise. The sample code is as follows:

	async function fn () {
        let p1 = await new Promise((resolve) => {
            console.log(1);
            resolve(2);
        });

        let p2 = await new Promise((resolve) => {
            console.log(p1);
            resolve(3);
        });

        let p3 = await new Promise((resolve) => {
            console.log(p2);
            resolve(4);
        });
        console.log(p3);
    }

    fn();
    
    Result is：
    1
    2
    3
    4
    
Async and Await further simplify the amount of code, optimize the writing, and are by far it is the most commonly used asynchronous writing.

Note:

1. "Await" is followed by a method that returns a Promise object or creates a new Promise object.
2. "Await" must be placed in the function marked by "Async".
3. If you need to catch an exception, you can use "try {} catch (exception) {}" to write "await" into the code block after "try".
4. The "Async" function returns a Promise, and the status is "resolved".
5. "Await" can accept non-Promise object as the result of an await expression. But no matter what is behind await, it will block the execution of the internal code of the "async" function. If there is an external code at this time, the external code is executed first, and then it returns to the internal execution.
6. If the code in async is synchronous, then it executes synchronously.


## Summary

Promise is almost finished, and also briefly talked about Async and Await. It feels that the most important part is to know how to use them. As for Generator, I have never heard of it before, and I feel that it is less used, so I'll look for a chance to talk about it later.

## References
[1] https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise

[2] https://www.jianshu.com/p/1ab01ee4102a

[3] https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/all
