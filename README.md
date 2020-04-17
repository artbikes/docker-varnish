# docker-varnish
My aim was to have a varnish instance running in-between Nginx container that does the routing for all incoming requests to the server and my WordPress container. With a carefully crafted Varnish configuration file I use the following to bring up the container:

```
docker run -d --name blog_benhall_varnish-2 
   --link blog_benhall-2:wordpress 
   -e VIRTUAL_HOST=blog.benhall.me.uk 
   -e VARNISH_BACKEND_PORT=80 
   -e VARNISH_BACKEND_HOST=wordpress 
   benhall/docker-varnish
```
The VIRTUAL_HOST environment variable is used for Nginx Proxy. The Docker link allowing Varnish and WordPress to communicate, my wordpress container is called blog_benhall-2. VARNISH_BACKEND_PORT defines the port WordPress runs on inside the container. VARNISH_BACKEND_HOST defines the internal hostname which we set while creating the docker link between containers.

When a request comes into the Varnish container it is either returned instantly or proxied to a different container and cached on the way back out.

Thanks to Nginx Proxy I didn’t have to change any configuration, as they simply reconfigured themselves as new containers were introduced. The setup really is a thing of beauty, that can now scale. I can use the same docker-varnish image to cache other containers in the future.
