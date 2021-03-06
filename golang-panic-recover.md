Title: Golang: Panic and recover
Tags: golang

When a panic("something") is called the program will exit. 

Just before doing so, all the functions in the stack are immediately exited just before calling all of the deferred statements in each.

                func letUsPanic() {
                        defer func(s string) {
                                fmt.Println("This will be called)
                        }()
                        panic("boom!")
                        fmt.Println("This will never be called")
                }

If the above was called from main(), then none of main's statements after letUsPanic() would be called either, except any deferred statements in main().

We can 'recover' from a panic, however, if we put a 'recover()' statement in a deferred statement.

                func letUsPanic() {
                        defer func() {
                                if e := recover(); e != nil {
                                        // e is the interface{} typed-value we passed to panic()
                                        fmt.Println("Whoops: ", e) // Prints "Whoops: boom!"
                                }
                        }()
                        panic("boom!")
                        fmt.Println("This will never be called")
                }                

Now if we run letUsPanic() from main(), the methods below letUsPanic() would be called.

It's bad style to use these. Go doesn't do exceptions as some languages do. Go uses errors and multiple return values.

This is mostly used, for example, at the root level in web services that should live forever, regardless of a *completley unexpected* panic occurring in a goroutine.
