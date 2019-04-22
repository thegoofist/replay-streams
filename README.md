# replay-streams
### _Boutade <thegoofist@protonmail.com>_

`replay-streams` let the programmer rewind an input stream to a previous point
so that the contents of the stream may be read again. This provides input
streams with a form of look-ahead.

When you instantiate a `replay-character-stream` you pass it an character input
stream to use as the underlying character source. You can then read from your
new string like a normal character input stream.

The `rewind` method will rewind the stream only once. If the stream has already
been rewound, `rewind` returns `nil`.

## Example

```

CL-USER> (ql:quickload 'replay-streams)    ;; NOT IN QUICKLISP. Install into quicklisp/local-projects/
To load "replay-streams":
  Load 1 ASDF system:
    replay-streams
; Loading "replay-streams"
[package replay-streams]
(REPLAY-STREAMS)

CL-USER> (use-package :replay-streams)
T

CL-USER> (defvar *source* (make-string-input-stream "Hey there here is a string!"))
*SOURCE*

CL-USER> (defvar *rpstream* (make-instance 'replay-character-stream 
                                           :source *source*))
                                           
CL-USER> (let ((buf (make-string 10)))
           (read-sequence buf *rpstream*)
           buf)
"Hey there "

CL-USER> (let ((buf (make-string 10)))
           (read-sequence buf *rpstream*)
           buf)
"here is a "

CL-USER> (rewind *rpstream*)
#<SB-IMPL::STRING-INPUT-STREAM {10042D0F83}>

CL-USER> (let ((buf (make-string 10)))
           (read-sequence buf *rpstream*)
           buf)
"Hey there "

CL-USER> (let ((buf (make-string 10)))
           (read-sequence buf *rpstream*)
           buf)
"here is a "

CL-USER> (let ((buf (make-string 10)))
           (read-sequence buf *rpstream*)
           buf)
"string!   "

CL-USER> (rewind *rpstream*)       ;; YOU CAN ONLY REWIND ONCE
NIL


```

