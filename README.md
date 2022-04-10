# E-voting-system-using-blockchain-and-python

This is the simple e-voting system, which is based on the implementation of Blockchain technology in Python. It covers the all the basic terms of blockchain and how to implement it on real-time application.


## What is blockchain? How it is implemented? And how it works?



## Instructions to run

Clone the project,

```sh
$ git clone https://github.com/satwikkansal/E-voting-system-using-blockchain-and-python.git
```

Install the dependencies,

```sh
$ cd E-voting-system-using-blockchain-and-python
$ pip install -r requirements.txt
```

Start a blockchain node server,

```sh
# Windows users can follow this: https://flask.palletsprojects.com/en/1.1.x/cli/#application-discovery
$ export FLASK_APP=service.py
$ flask run --port 8000
```

One instance of our blockchain node is now up and running at port 8000.


Run the application on a different terminal session,

```sh
$ python app.py
```

The application should be up and running at [http://localhost:5000](http://localhost:5000).

Here are a few screenshots

1. Posting vote

![image.png](https://github.com/adhikarir/E-voting-system-using-blockchain-and-python/blob/master/screenshots/1.png)

2. Requesting the node to mine

![image.png](https://github.com/adhikarir/E-voting-system-using-blockchain-and-python/blob/master/screenshots/2.png)

3. Resyncing with the chain for updated data

![image.png](https://github.com/adhikarir/E-voting-system-using-blockchain-and-python/blob/master/screenshots/3.png)

4. Chain of the transaction

![image.png](https://github.com/adhikarir/E-voting-system-using-blockchain-and-python/blob/master/screenshots/4.png)

4. App screenshot

![image.png](https://github.com/adhikarir/E-voting-system-using-blockchain-and-python/blob/master/screenshots/5.png)

To play around by spinning off multiple custom nodes, use the `register_with/` endpoint to register a new node. 

Here's a sample scenario that you might wanna try,

```sh
# Make sure you set the FLASK_APP environment variable to service.py before running these nodes
# already running
$ flask run --port 8000 &
# spinning up new nodes
$ flask run --port 8001 &
$ flask run --port 8002 &
```

You can use the following cURL requests to register the nodes at port `8001` and `8002` with the already running `8000`.

```sh
curl -X POST \
  http://127.0.0.1:8001/register_with \
  -H 'Content-Type: application/json' \
  -d '{"node_address": "http://127.0.0.1:8000"}'
```

```sh
curl -X POST \
  http://127.0.0.1:8002/register_with \
  -H 'Content-Type: application/json' \
  -d '{"node_address": "http://127.0.0.1:8000"}'
```

This will make the node at port 8000 aware of the nodes at port 8001 and 8002, and make the newer nodes sync the chain with the node 8000, so that they are able to actively participate in the mining process post registration.

To update the node with which the frontend application syncs (default is localhost port 8000), change `CONNECTED_NODE_ADDRESS` field in the [views.py](/app/views.py) file.

Once you do all this, you can run the application, create transactions (post messages via the web inteface), and once you mine the transactions, all the nodes in the network will update the chain. The chain of the nodes can also be inspected by inovking `/chain` endpoint using cURL.

```sh
$ curl -X GET http://localhost:8001/chain
$ curl -X GET http://localhost:8002/chain
```
