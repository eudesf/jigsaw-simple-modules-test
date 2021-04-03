# Jigsaw Modules Test

Simple project to test Java modules compilation.

There are two sample modules for testing ```product``` and ```order```, in which:

```order``` requires ```product```

So we must build ```product``` module first:

```bash
# creating necessary directories
mkdir -p mods classes/product classes/order

# creating JAR for product module
javac -d classes/product $(find product/src -iname "*.java")
jar --create --file mods/product.jar --main-class com.modulestest.product.Main -C classes/product .

# Running product module
java --module-path mods --module product

# creating JAR for order module
javac -d classes/order --module-path mods $(find order/src -iname "*.java")
jar --create --file mods/order.jar --main-class com.modulestest.order.Main -C classes/order .

# Running order module
java --module-path mods --module order
```

After this, both modules can run just by changing the module name in ```--module``` param.

Notice that we needed to include the ```--module-path``` param in the javac of ```order``` module. 
This is necessary because ```order``` depends on ```product``` and it will look for the ```product``` 
module in the specified module path. 
