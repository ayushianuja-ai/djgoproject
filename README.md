# Django Signals
#By default, Django signals are executed synchronously. This means that when a signal is sent, the receiver functions are executed in the same process and thread as the sender, and the sender will wait for the receiver to finish before continuing.

 models.py
from django.db import models
from django.db.models.signals import post_save
from django.dispatch import receiver
import time

class MyModel(models.Model):
    name = models.CharField(max_length=100)

@receiver(post_save, sender=MyModel)
def my_model_post_save(sender, instance, created, **kwargs):
    print(f"Signal handler started for {instance.name}")
    time.sleep(3)  # Simulate a long-running task
    print(f"Signal handler finished for {instance.name}")

from django.shortcuts import render
from .models import MyModel

def create_model(request):
    # This will trigger the signal
    my_instance = MyModel.objects.create(name='Test Model')
    print("Model instance created")  # Will print before the signal handler finishes
    return render(request, 'template.html')
