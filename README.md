# YAKVS [![Build Status](https://travis-ci.org/sci4me/yakvs.svg?branch=master)](https://travis-ci.org/sci4me/yakvs) [![GoDoc](https://godoc.org/github.com/google/go-github/github?status.svg)](https://godoc.org/github.com/sci4me/yakvs) [![GoSearch](http://go-search.org/badge?id=github.com%2Fsci4me%2Fyakvs)](http://go-search.org/view?id=github.com%2Fsci4me%2Fyakvs)

YAKVS (Yet Another Key Value Store) is a tiny, lightweight, networked, in-memory key-value store written in Go.

## Install

    go get github.com/sci4me/yakvs

## Usage

    yakvs 

If YAKVS is being run for the first time, it will automatically generate the default configuration file in `yakvs.conf` and start running with those options.

## Protocol

YAKVS uses a text-based TCP protocol. Commands and results are newline delimited. Here are the supported commands:

    PUT <key> <value>
    Associates the specified value with the specified key in the store
    Returns: 
      OK - success
      ERROR - incorrect arguments

    GET <key>
    Returns the value to which the specified key is mapped
    Returns:
      VALUE - the value to which the specified key is mapped
      nil - store contains no mapping for the key
      ERROR - incorrect arguments

    HASKEY <key>
    Returns true if this store contains a mapping for the specified key
    Returns:
      TRUE - store contains a mapping for the key
      FALSE - store contains no mapping for the key
      ERROR - incorrect arguments

    HASVALUE <value>
    Returns true if this store contains a mapping for the specified value
    Returns:
      TRUE - store contains a mapping for the value
      FALSE - store contains no mapping for the value
      ERROR - incorrect arguments

    REMOVE <key>
    Removes the mapping for the specified key from this map if present
    Returns:
      OK - success
      ERROR - incorrect arguments

    SIZE
    Returns the number of key-value mappings in this store
    Returns:
      SIZE - the number of key-value mappings in this store
      ERROR - too many arguments

    CLEAR
    Removes all of the mappings from this store
    Returns:
      OK - success
      ERROR - too many arguments

    LIST
    Lists the key-value mappings in this store
    Returns:
      KVPs - the key-value mappings in the store
      nil - there are no key-value mappings in the store
      ERROR - too many arguments

    LIST KEYS
    Lists the keys in this store
    Returns:
      KEYS - the keys in the store
      nil - there are no key-value mappings in the store
      ERROR - incorrect arguments

    LIST VALUES
    Lists the values in this store
    Returns:
      VALUES - the values in the store
      nil - there are no key-value mappings in the store
      ERROR - incorrect arguments

    QUIT
    Closes the connection
    Returns:
      BYE - success
      ERROR - incorrect arguments

If the server is stopped while there are active connections, the following message will be sent to the connections:
```
SERVER STOPPED
```
The connections will then be closed. All connections will be closed in this fashion before the server stops.

If a connection is attempted while the number of clients connected to the server is equal to the maximum number of clients, the connecting client will receive the following message:
```
CONNECTION REFUSED
```
The connection will then be closed.

If connection timeout is enabled and a connection goes too long (the timeout setting) without issuing a command, the connection will receive the following message:
```
TIMED OUT
```
The connection will then be closed.