Memo - Design
============

Few questions and points
------------------------

1. We need a to start a daemon - trivial
2. We need to have locking at the lowest level possible for high performance
3. Shared memory vs allocating as need? From description, it is not clear



Class MemoServer - A multithreaded TCP server 
---------------------------------------------
```
/*
This class is the starting point.
It creates a cache instance and a TCP socket. 
It then goes on to an infinite loop that dispatches 
the task of parsing and handling a request to a 
newly created thread.
Enhancements:
1. Thread pooling
*/
main(argc, argv) {
  mem_size = argv[memory_size]
  replacement_algorithm = argv[replacement_algorithm]
  Cache *cache;
  
  switch(algorithm) {
   case LRU:
        cache = new LRUCache(mem_size); break;
   case random:
        cache = new RandomCache(mem_size); break;
   case landlord:
        cache = new LandlordCache(mem_size); break;
   default:
        throw IllegalAlgorithmException;
  }
  
  create a TCP socket
  while (1) {
    accept a connection
    create a thread to read and handle request
  }
}

threadHandler(fileDescriptor) {
  data = read(fileDescriptor)
  Command command = new Command(data);
  response = command.execute();
  write response back to the fileDescriptor
}
```
Class Command
-------------
```
/*
This class reads the commandData to a command object.
The execute method simply executes the command - making accesses to the cache as required
Enhancements:
1. Create subclasses for each command if necessary, or for each type like storageCommand, retrievalCommand, statsCommand etc
*/
Command(commandData) {
  Parse commandData and store the following to class variables
  CommandType //(set, add, replace, append, prepend, etc..)
  key //(the key being accessed)
  value //(the value for that key)
  expirationTime //if required
  flags //if required
  .. //any other data as required by the command
}

execute() {
  Use the parsed data to make accesses to the cache
  cache.put(key, value [,metadata])
  cache.get(key)
  Respond result appropriately
}
```
Class Cache
-----------
```
/*
This class instantiates a shared memory region and allows
child classes to implement a key-value store
*/
Cache(size) {
  startAddress = acquire memory of "size" at once
  
}

get(key) // Child classes need to implement
put(key, value [,metadata]) // 
```
Class LRUCache (extends Cache)
------------------------------

```
put(key, value, metadata) {

}

get() {

}
```

Class RandomCache (extends Cache)
---------------------------------
```
put(key, value, metadata) {

}

get() {

}
```

Class LandlordCache (extends Cache)
-----------------------------------

```
put(key, value, metadata) {

}

get() {

}
```
