# Logging

Totoval provide an easy use logging helpers with log level supported.

## Packages
* github.com/totoval/framework/helpers/log
* github.com/totoval/framework/helpers/toto

## Usage

```go
    log.Info("this is the message", toto.V{"key1": value1, "key2": value2})
    log.Warn("this is the message", toto.V{"key1": value1, "key2": value2})
    log.Fatal("this is the message", toto.V{"key1": value1, "key2": value2})
    log.Debug("this is the message", toto.V{"key1": value1, "key2": value2})
    log.Panic("this is the message", toto.V{"key1": value1, "key2": value2})
    log.Trace("this is the message", toto.V{"key1": value1, "key2": value2})
    log.Error(errors.New("this is the error message"), toto.V{"key1": value1, "key2": value2})
```