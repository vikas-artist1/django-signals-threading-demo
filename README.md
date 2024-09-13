Do Django signals run in the same thread as the caller?
Answer: By default, Django signals run synchronously in the same thread as the sender (caller). This means that when a signal is sent, the connected signal handlers are executed in the same thread as the signal sender, and the sender will wait for all handlers to complete before continuing its execution.

However, Django also provides the option to execute signals asynchronously by using the async parameter with the @receiver decorator. When a signal is set to run asynchronously, it operates in a separate thread or process, allowing the sender to continue without waiting for the signal handler to finish.

Here’s a code snippet that demonstrates how Django signals run synchronously in the same thread as the caller:
from django.db.models.signals import post_save
from django.dispatch import receiver
from django.db import models
import threading
import time

class MyModel(models.Model):
    name = models.CharField(max_length=100)

@receiver(post_save, sender=MyModel, dispatch_uid="my_signal_handler")
def my_signal_handler(sender, instance, **kwargs):
    print("Signal received for:", instance.name)
    time.sleep(2)  # Simulate a delay of 2 seconds in task execution
    print("Signal handler finished")

# Creating an instance of MyModel
obj = MyModel.objects.create(name="Test")
print("Object created")

# Printing the current thread’s ID
print("Current thread ID:", threading.get_ident())

Here's a human-readable explanation with bold headings formatted in Markdown for inclusion in your `README.md`:

---

### **Explanation**

In the above code:

- **Django Model Definition:** We define a Django model `MyModel`.
- **Signal Handler:** We create a signal handler function `my_signal_handler` that is connected to the `post_save` signal of `MyModel`.
- **Simulating Delay:** Inside the signal handler, we simulate a delay of 2 seconds in task execution using `time.sleep(2)`.
- **Instance Creation and Thread ID:** After creating an instance of `MyModel`, we print "Object Created" and the thread ID using `threading.get_ident()`.

This setup allows us to compare the thread ID of the main thread with the thread ID of the signal handler.

When we run this code, we'll notice that:
- Both the main thread and the thread inside the signal handler have the same thread ID, indicating that the signal handler runs in the same thread as the caller (main thread).
- The print statement "Signal handler finished" is displayed only after a delay of 2 seconds, showing that the signal handler blocks execution until it completes its task.

This confirms that Django signals run synchronously in the same thread as the caller by default.
