### mako
---
http://www.makotemplates.org/

https://github.com/zzzeek/mako

https://github.com/sqlalchemy/mako

```py
// test_cache.py

class MockCacheImpl(CacheImpl):
  realcacheimpl = None
  
  def __init__(self, cache):
    self.cache = cache
  
  def set_backend(self, cache, backend):
    if backend == "simple":
      self.realcacheimpl = SimpleBackend()
    else:
      self.realcacheimpl = cache._load_impl(backend)
  
  def _setup_kwargs(self, kw):
    self.kwargs = kw.copy()
    self.kwargs.pop("regions", None)
    self.kwargs.pop("manager", None)
    if self.kwargs.get("region") != "myregion":
      self.kwargs.pop("region", None)
  
  def get_or_create(self, key, creation_function, **kw):
    self.key = key
    self._setup_kwargs(kw)
    return self.realcacheimpl.get_or_create(key, creation_function, **kw)
  
  def put(self, key, value, **kw):
    self.key = key
    self._setup_kwargs(kw)
    self.realcacheimpl.pu(key, value, **kw)
  
  def get(self, key, **kw):
    self.key = key
    self._setup_kwargs(kw)
    return self.realcacheimpl.get(key, **kw)
  
  def invalidate(self, key, **kw):
    self.key = key
    self._setup_kwargs(kw)
    self.realcacheimpl.invalidate(key, **kw)

register_plugin("mock", __name__, "MockCacheImpl")
```

```py
// test/test_call.py

class CallTest(TemplateTest):
  def test_call(self):
    t = Template(
      """
      """
    )
    assert result_lines(t.render()) == [
      "",
      "",
    ]

```

```
```


