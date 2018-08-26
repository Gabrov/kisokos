Docker for Windows futtatásakor a docker konténerek belső Hyper-V-s hálózata nem érhető el alapból. Be kell állítani a route-olást
a Hyper-V alapértelmezett és privát adaptere között:

```
route /P add 172.0.0.0 MASK 255.0.0.0 10.0.75.2
```