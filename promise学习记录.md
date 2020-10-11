当使用promise.all方法的时候，如果每个promise方法作了catch处理，因为catch也是一个promise，catch相当于处理成功，所以all函数可以执行成功

![image-20200915174455847](/Users/yangyang/Library/Application Support/typora-user-images/image-20200915174455847.png)

promise.race是哪个promise返回的快，用哪个promise的返回结果

promise队列（map和reduce都可以实现）

![image-20200922142849256](/Users/yangyang/Library/Application Support/typora-user-images/image-20200922142849256.png)

![image-20200922142938624](/Users/yangyang/Library/Application Support/typora-user-images/image-20200922142938624.png)



就是在.then方法里面return一个新的promise，然后利用后面的then方法处理return出来的promise返回，这样就有了执行先后顺序，形成了队列执行方式

