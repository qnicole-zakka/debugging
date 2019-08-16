### Reminder to install flask and pillow are in README files inside of "benign" and "malicious" directories. But not in the scripts. Users need to install these two before "make run".

## Make run with "GLIBC_2.27 not found" error

```bash
xi@xiqin-desk1:~/Downloads/openvino-graphene-demo-master$ make run
sudo echo "Running native..."
Running native...
cd backend && ./openvino_native.sh &
cd stolen  && sudo ./attack_object_detector &
cd frontend/benign    && DEMO_SGX=0 FLASK_APP=run.py flask run -p 5000 &
cd frontend/malicious && DEMO_SGX=0 FLASK_APP=run.py flask run -p 5001 &
xi@xiqin-desk1:~/Downloads/openvino-graphene-demo-master$ Watching for process object_detection_sample_ssd_pp
Watching for process object_detection_sample_ssd_loop
Watching for process pal-Linux-SGX
 * Serving Flask app "run.py"
 * Environment: production
   WARNING: This is a development server. Do not use it in a production deployment.
   Use a production WSGI server instead.
 * Debug mode: off
 * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
 * Serving Flask app "run.py"
 * Environment: production
   WARNING: This is a development server. Do not use it in a production deployment.
   Use a production WSGI server instead.
 * Debug mode: off
 * Running on http://127.0.0.1:5001/ (Press CTRL+C to quit)
/home/openvino/object_detection_sample_ssd_loop: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.27' not found (required by /lib/x86_64-linux-gnu/libcpu_extension.so)
/home/openvino/object_detection_sample_ssd_loop: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.27' not found (required by /lib/x86_64-linux-gnu/libinference_engine.so)
/home/openvino/object_detection_sample_ssd_loop: /usr/lib/x86_64-linux-gnu/libstdc++.so.6: version `GLIBCXX_3.4.22' not found (required by /lib/x86_64-linux-gnu/libinference_engine.so)
/home/openvino/object_detection_sample_ssd_loop: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.27' not found (required by /lib/x86_64-linux-gnu/libopencv_imgproc.so.4.1)
/home/openvino/object_detection_sample_ssd_loop: /lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.27' not found (required by /lib/x86_64-linux-gnu/libopencv_core.so.4.1)
127.0.0.1 - - [16/Aug/2019 11:21:51] "GET / HTTP/1.1" 200 -
127.0.0.1 - - [16/Aug/2019 11:21:51] "GET /static/bootstrap.min.css HTTP/1.1" 200 -
127.0.0.1 - - [16/Aug/2019 11:21:51] "GET /static/jquery.min.js HTTP/1.1" 200 -
127.0.0.1 - - [16/Aug/2019 11:21:51] "GET /favicon.ico HTTP/1.1" 404 -
```

## Make run: Socket connection error when performing inference
```bash
127.0.0.1 - - [16/Aug/2019 11:35:04] "GET /_show_image?name=airport.jpg HTTP/1.1" 200 -
[2019-08-16 11:35:07,901] ERROR in app: Exception on /_show_predict [GET]
Traceback (most recent call last):
  File "/home/xi/.local/lib/python2.7/site-packages/flask/app.py", line 2446, in wsgi_app
    response = self.full_dispatch_request()
  File "/home/xi/.local/lib/python2.7/site-packages/flask/app.py", line 1951, in full_dispatch_request
    rv = self.handle_user_exception(e)
  File "/home/xi/.local/lib/python2.7/site-packages/flask/app.py", line 1820, in handle_user_exception
    reraise(exc_type, exc_value, tb)
  File "/home/xi/.local/lib/python2.7/site-packages/flask/app.py", line 1949, in full_dispatch_request
    rv = self.dispatch_request()
  File "/home/xi/.local/lib/python2.7/site-packages/flask/app.py", line 1935, in dispatch_request
    return self.view_functions[rule.endpoint](**req.view_args)
  File "/home/xi/Downloads/openvino-graphene-demo-master/frontend/benign/run.py", line 59, in show_predict
    result = _gen_predict(img_path)
  File "/home/xi/Downloads/openvino-graphene-demo-master/frontend/benign/run.py", line 23, in _gen_predict
    client_socket.connect(('127.0.0.1', 10000))
  File "/usr/lib/python2.7/socket.py", line 228, in meth
    return getattr(self._sock,name)(*args)
error: [Errno 111] Connection refused
127.0.0.1 - - [16/Aug/2019 11:35:07] "GET /_show_predict?name=airport.jpg HTTP/1.1" 500 -
```

## "Make stop" error (make stop and make stopsgx similarly)
I'll have to kill process manually
```bash
make stopsgx
sudo echo "Stopping sgx..."
Stopping sgx...
cd backend && ./openvino_graphene.sh stop
"docker stop" requires at least 1 argument.
See 'docker stop --help'.

Usage:  docker stop [OPTIONS] CONTAINER [CONTAINER...]

Stop one or more running containers
Makefile:34: recipe for target 'stopsgx' failed
make: *** [stopsgx] Error 1

```

## "Make runsgx": "/dev/gsgx no such file" error
```bash
xi@xiqin-desk1:~/Downloads/openvino-graphene-demo-master$ make runsgx
sudo echo "Running sgx..."
Running sgx...
cd backend && ./openvino_graphene.sh &
cd stolen  && sudo ./attack_object_detector &
cd frontend/benign    && DEMO_SGX=1 FLASK_APP=run.py flask run -p 5000 &
cd frontend/malicious && DEMO_SGX=1 FLASK_APP=run.py flask run -p 5001 &
xi@xiqin-desk1:~/Downloads/openvino-graphene-demo-master$ Watching for process object_detection_sample_ssd_pp
Watching for process object_detection_sample_ssd_loop
Watching for process pal-Linux-SGX
docker: Error response from daemon: linux runtime spec devices: error gathering device information while adding custom device "/dev/gsgx": no such file or directory.
ERRO[0000] error waiting for container: context canceled 
 * Serving Flask app "run.py"
 * Environment: production
   WARNING: This is a development server. Do not use it in a production deployment.
   Use a production WSGI server instead.
 * Debug mode: off
 * Running on http://127.0.0.1:5001/ (Press CTRL+C to quit)
 * Serving Flask app "run.py"
 * Environment: production
   WARNING: This is a development server. Do not use it in a production deployment.
   Use a production WSGI server instead.
 * Debug mode: off
 * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
```

