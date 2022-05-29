
I followed the code from [https://github.com/oscarknagg/few-shot](https://github.com/oscarknagg/few-shot)


0. My server
MY OS
Ubuntu 18.04LTS

1. git clone

~~~
git clone https://github.com/oscarknagg/few-shot.git
~~~



### Process start from git clone

1. need to edit requirement.txt

MarkupSafe==1.0  -> MarkupSafe==1.1.0
pkg-resources==0.0.0  -> #pkg-resources==0.0.0
torch==1.0.0 -> torch

if something is not working.. follow below blog's instruction. 

https://exerror.com/note-this-error-originates-from-a-subprocess-and-is-likely-not-a-problem-with-pip/
