# Chapter 9 IO and Conduit

### Typeable Type Class, pg 325
* allows for runtime type checking
* Data.Dynamic can be used to model dynamically typed values that can be cast at runtime

### Files
* `withFile` is a good abstraction for handling file handles, they are automatically closed when the handle "goes out of scope"
    * kinda like Python's `with open(path) as f:` statement
* `</>` operator can be used to join file paths in a way that is cross platform, something like `myPath </> filename == /something/my/mpath/filename`
* similarly, there is a `<.>` operator that can be used to add an extension to a path

### Conduit
* based on `actors`
    * `sources` provide values to be consumed
        * get text from a file
        * read from the network
        * getting items from a list
    * `sinks` consume a stream of data from `sources`
        * may produce a final vale at the end, which is a reduction of the many values into some sort of sum
            * could also be writing results to a disk
    * `transformers` (formally known as "conduits") consume an output stream and create another output stream from the derived data
* the important type is `ConduitT i o m r` (it is a monad transformer)
    * `i` type of values in input stream
    * `o` type of values in output stream
        * conduits who don't produce output need to have 'Void' as their `o` type
    * `m` monad for side effects from the stream, could be IO
    * `r` type of the final result
* `MonadIO` is nice to use as the `m` type because instead of making it so that the code only accepts the IO monad, it makes it so that the Conduit code can accept any monad stack that is capable of IO
    ```haskell
    myConduitProgram :: (Monad m, MonadIO m) => ConduitT BS.ByteString BS.ByteString m ()
    ```
    * in this way, you can do IO inside the conduit by using `liftIO` inside the do notation
* operators
    * `.|` this pipe-looking operator is used to connect conduits together
* runner functions
    * `runConduit` or `runConduitPure` are used to run the conduit program (it's a monad transformer, I think those usually have a function that you have to use to deal with their types)
    * `runConduitRes` if the Conduit has access to a resource like a file or a database connection, you need to use this function
* file operations can be done using Conduit's `sourceFile`, `sourceHandle`, `sinkFile`, `sinkHandle` (pg 332)
