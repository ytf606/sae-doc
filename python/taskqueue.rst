.. include:: ../services/taskqueue.rst

API使用手册
==============

.. module:: sae.taskqueue

.. py:function:: add_task(queue_name, url, payload=None, **kws)

   快速添加任务    

   queue_name: 任务队列的名称

   url: 任务的url，如： /tasks/task_name

   payload: 可选，如果payload存在且不为None，则该任务为一POST任务，payload会作为请求
   的POST的数据。

   delay/prior: 可选，使用方法参见Task。


.. py:class:: Task(url, payload=None, **kwargs)

   Task类
     
   url: 任务的url，如： /tasks/task_name

   payload: 可选, 如果payload存在且不为None，则该任务为一POST任务，payload会作为请求
   的POST的数据。

   delay: 可选，设置任务延迟执行的时间，单位为秒，最大可以为600秒。

   prior: 可选，如果设置为True，则任务会被添加到任务队列的头部。
 
.. py:class:: TaskQueue(name, auth_token=None)

   TaskQueue类

   name: 任务队列的名称。

   auth_token: 可选, 一个包含两个元素的元组 (access_key, secretkey_key)。
    
   .. py:method:: add(task)

      添加一个任务
          
      task: 添加的任务，可以为单个Task任务，也可以是一个Task列表。

   .. py:method:: size()

      获取当前队列中还有多少未执行的任务。

使用示例
===========

1. 添加一个任务。   ::
    
    from sae.taskqueue import Task, TaskQueue

    queue = TaskQueue('queue_name')
    queue.add(Task("/tasks/foo"))

2. 添加一个POST任务。   ::

    queue.add(Task("/tasks/bar", "data"))

3. 批量添加任务。   ::

    tasks = [Task("/tasks/update", user) for user in users]
    queue.add(tasks)

4. 快速添加任务。   ::

    from sae.taskqueue import add_task
    add_task('queue_name', '/tasks/push', 'msg')

..  note:: 

    任务的url现在已经改为相对的url，目前兼容绝对url，但是不推荐使用。 
    任务默认使用GET方式请求，如果Task带有payload参数且不为None则使用POST方式请求。
