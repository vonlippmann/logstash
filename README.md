# Log Stash
Push json formatted log to local file system

## Usage
1. go get github.com/vonlippmann/logstash
2. import "github.com/vonlippmann/logstash" in your code
3. make a new instance of Logger:
```go
logger := NewLogStash(&Config{
        LogPath:     "<your specified path>",
        LogKeepDays: 0,
        FileName:    "<your specified file name>",
        CleanLog:    false,
})
```

4. then you can use logger in anywhere you need sink log to the path you specified by this command:
```go
logger.Sink(Massage{
    "auth": "fengjiabin",
    "cop":  "xiaomi",
    })
``` 
### Hooks
1. you can use hook to post process the massage you sinked, such as add some other field. you can do this simply by register a hook function:
```go
logger.RegisterHook(func(msg Massage)(err error){
    mas["email"]="j.b.feng@foxmail.com"
})
```

### Example

```go
package main

var logger *Logger

func main(){
    for{
         logger.Sink(Massage{
            "auth": "fengjiabin",
            "cop":  "xiaomi",
        })
    }
}

func addField(massage Massage) (err error) {
    massage["email"] = "j.b.feng@foxmail.com"
    return
}

func init() {
    logger = NewLogStash(&Config{
        LogPath:     "/var/log/logstash",
        LogKeepDays: 0,
        FileName:    "elk",
        CleanLog:    false,
    })
    logger.RegisterHook(addField)
}

```
