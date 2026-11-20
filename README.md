# databasehashapp

a file-based hash bucket system.

this project stores strings in bucket files using the FNV-1a hashing algorithm.

when a bucket gets full, the program creates an overflow file. an index file keeps track of the latest overflow file.