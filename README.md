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
