---
title:  "[Tensorflow 에러해결] self._traceback = tf_stack.extract_stack_for_node(self._c_op)"
excerpt: "tf_stack_.extract...."

categories:
  - etc
tags: [tensorflow, study, pytorch, etc]
classes: wide

last_modified_at: 2021-11-09T10:40:00-05:00
---


갑자기 잘 되던 텐서가 다음과 같은 에러를 뿜어냈다. 😱😱 에러가 너무 길어서 식겁했는데 생각보다 간단한 문제였다.

~~~
  File "C:\Users\ccaa9\Anaconda3\envs\emotion_fan\lib\site-packages\tensorflow\python\client\session.py", line 1394, in _do_call
    raise type(e)(node_def, op, message)
tensorflow.python.framework.errors_impl.UnknownError: 2 root error(s) found.
  (0) Unknown: Failed to get convolution algorithm. This is probably because cuDNN failed to initialize, so try looking to see if a warning log message was printed above.
    [[node pnet/conv1/Conv2D (defined at \PycharmProjects\real-time_recognition\face-emotion-recognition\src\facial_analysis.py:42) ]]
    [[pnet/prob1/_5]]
  (1) Unknown: Failed to get convolution algorithm. This is probably because cuDNN failed to initialize, so try looking to see if a warning log message was printed above.
    [[node pnet/conv1/Conv2D (defined at \PycharmProjects\real-time_recognition\face-emotion-recognition\src\facial_analysis.py:42) ]]
0 successful operations.
0 derived errors ignored.

Original stack trace for 'pnet/conv1/Conv2D':
  File "\PycharmProjects\real-time_recognition\face-emotion-recognition\src\test_fer.py", line 41, in <module>
    imgProcessing=FacialImageProcessing(False)
  File "\PycharmProjects\real-time_recognition\face-emotion-recognition\src\facial_analysis.py", line 42, in __init__
    tf.import_graph_def(FacialImageProcessing.load_graph_def(model_file), name=model_files[model_file])
  File "\Anaconda3\envs\emotion_fan\lib\site-packages\tensorflow\python\util\deprecation.py", line 535, in new_func
    return func(*args, **kwargs)
  File "\Anaconda3\envs\emotion_fan\lib\site-packages\tensorflow\python\framework\importer.py", line 400, in import_graph_def
    return _import_graph_def_internal(
  File "\Anaconda3\envs\emotion_fan\lib\site-packages\tensorflow\python\framework\importer.py", line 513, in _import_graph_def_internal
    _ProcessNewOps(graph)
  File "\Anaconda3\envs\emotion_fan\lib\site-packages\tensorflow\python\framework\importer.py", line 243, in _ProcessNewOps
    for new_op in graph._add_new_tf_operations(compute_devices=False):  # pylint: disable=protected-access
  File "\Anaconda3\envs\emotion_fan\lib\site-packages\tensorflow\python\framework\ops.py", line 3707, in _add_new_tf_operations
    new_ops = [
  File "\Anaconda3\envs\emotion_fan\lib\site-packages\tensorflow\python\framework\ops.py", line 3708, in <listcomp>
    self._create_op_from_tf_operation(c_op, compute_device=compute_devices)
  File "\Anaconda3\envs\emotion_fan\lib\site-packages\tensorflow\python\framework\ops.py", line 3590, in _create_op_from_tf_operation
    ret = Operation(c_op, self)
  File "\Anaconda3\envs\emotion_fan\lib\site-packages\tensorflow\python\framework\ops.py", line 2045, in __init__
    self._traceback = tf_stack.extract_stack_for_node(self._c_op)


Process finished with exit code 1
~~~

pytorch를 쓰지 않는데 import 하고 있어서  tensorflow 와 충돌? 비스무리하게 난것 같다. 

바로 import torch, import torchvision 을 주석처리해주었다.

해결..  끝!



난또 무슨 프로세스랑  gpu 메모리 문젠줄 알고.. 코드를 더 넣어줘야하나~ process  를 kill 해야하나~ 했네 ㅋㅋㅋㅋ 간단히 해결해서 다행이다..! 빡코딩!!

