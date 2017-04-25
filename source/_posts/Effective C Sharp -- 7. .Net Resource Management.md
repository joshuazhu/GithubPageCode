---
 title: Effective C# -- 7. .Net Resource Management
 date: {date}
 categories: .Net
---

Two mechanisms helps developers control the lifetime of unmanaged resources: __finalizers__ and the __IDisposable__ interface.
  * Finalizer: ensures your objects always have a way to release unmanaged resources (__avoid resource leaks__).
    * Called by GC, will be called after an object becomes garbage.
    *  Execute at nondetermin- istic times, so __minimize__ the need for __creating__ finalizers, and also __minimize__ the __need for executing_ the finalizers that do exist
    * Relying on finalizers also affects performance: GC __cannot remove__ an object if it __requires finalization__.
    * Finalizers and GC are in __different threads__: GC put objects that need finalization into a queue, then another thread will execute all the finalizers.
  * IDisposable: An interface that provides a less intrusive way to return resources to the system in a timely manner (__avoids the performance drain on the Garbage Collector that final- izers introduce__)