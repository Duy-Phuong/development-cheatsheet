

[How to base64 encode and decode from command-line - Serverlab](https://www.serverlab.ca/tutorials/linux/administration-linux/how-to-base64-encode-and-decode-from-command-line/)

# How to base64 encode and decode from command-line

## Overiew

In this tutorial, you will learn how to base64 encode and decode from the command-line on Linux. You will also learn what base64 encoding is and why it should never be used to protect data from unauthorized access.

Base64 encoding and decoding data has many use cases. One being is ensuring data integrity when transferring data over the network, while another is storing Secrets in Kubernetes.

After reading this tutorial you will understand how to easily encode files or strings, and then decode them back.

## How to base64 encode on Ubuntu, Debian, OSX, and Red Hat

If you are running popular linux distributions, such as Ubuntu, Debian, CentOS, or Red Hat, the base64 command-line tool is typically pre-installed. You should not have to perform any additional steps.

OSX also comes bundled with its own version of base64.

## Why Base64 Encode Data

Transferring an ASCII file over the network can cause corruption if not decoded correctly. The reason is ASCII files are string converted to bytes, and when those bytes are decoded incorrectly back to ASCII your data becomes corrupt.

Base64 was introduced as a way to convert your ASCII data into arbitrary bytes, where they could then be transferred as bytes, and decoded correctly back to ASCII.

In short, base64 encoding ensures the integrity of our data when transferred over the network.

## Base64 is not Encryption

Encoding files is not encryption and should never be used to secure sensitive data on disk. Rather it is a useful way of transferring or storing large data in the form of a string.

While it may obfuscate that actual data from should surfers, anyone who has access to base64 encoded data can easily decode it.

## Base64 Encoding a String

To base64 encode string you can pipe an echo command into the base64 command-line tool. To ensure no extra, hidden characters are added use the `-n` flag.

Without the `-n` flag you may capture a hidden characters, like line returns or spaces, which will corrupt your base64 encoding.

```bash
echo -n 'my-string' | base64
```

Which will output the following

```bash
bXktc3RyaW5n
```

## Base64 Encoding a File

To base64 encode a file

```bash
base64 /path/to/file
```

This will output a very long, base64 encoded string. You may want to write the stdout to file instead.

```none
bas64 /path/to/file > output.txt
```

## Decoding Strings

To decode with base64 you need to use the `--decode` flag. With encoded string, you can pipe an echo command into base64 as you did to encode it.

Using the example encoding shown above, let’s decode it back into its original form.

```none
echo -n 'bXktc3RyaW5n' | base64 --decode
```

Provided your encoding was not corrupted the output should be your original string.

## Decoding Files

To decode a file with contents that are base64 encoded, you simply provide the path of the file with the `--decode` flag.

```none
base64 --decode /path/to/file
```

As with encoding files, the output will be a very long string of the original file. You may want to output stdout directly to a file.

```none
base64 --decode /path/to/file > output.txt
```

## Conclusion

In this tutorial, you learned how to base64 encode files and strings. This something commonly done to transfer files in such a way that it remains
