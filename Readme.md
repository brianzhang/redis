## redis.go

redis.go is a client for the [redis](http://github.com/antirez/redis) key-value store. 

This library inherits from [hoisie/redis](https://github.com/hoisie/redis)

Add some features:

* Support ZCOUNT, ZRANGE and ZREVRANGE with socres
* Export ErrNil for not existing key error

This library is stable and is used in production environments. However, some commands have not been tested as thoroughly as others. If you find any bugs please file an issue!

# Installation

Just run `go get github.com/ianzeng/redis`

## Examples

Most of the examples connect to a redis database running in the default port -- 6367. 


### Hello World example

```go
package main

import "github.com/ianzeng/redis"

func main() {
    var client redis.Client
    var key = "hello"
    client.Set(key, []byte("world"))
    val, _ := client.Get("hello")
    println(key, string(val))
}
```

### Strings 

    var client redis.Client
    client.Set("a", []byte("hello"))
    val, _ := client.Get("a")
    println(string(val))
    client.Del("a")

### Lists

    var client redis.Client
    vals := []string{"a", "b", "c", "d", "e"}
    for _, v := range vals {
        client.Rpush("l", []byte(v))
    }
    dbvals,_ := client.Lrange("l", 0, 4)
    for i, v := range dbvals {
        println(i,":",string(v))
    }
    client.Del("l")

### Publish/Subscribe
    sub := make(chan string, 1)
    sub <- "foo"
    messages := make(chan Message, 0)
    go client.Subscribe(sub, nil, nil, nil, messages)

    time.Sleep(10 * 1000 * 1000)
    client.Publish("foo", []byte("bar"))

    msg := <-messages
    println("received from:", msg.Channel, " message:", string(msg.Message))

    close(sub)
    close(messages)


More examples coming soon. See `redis_test.go` for more usage examples.

## Commands not supported yet

* MULTI/EXEC/DISCARD/WATCH/UNWATCH
* SORT
* ZUNIONSTORE / ZINTERSTORE

