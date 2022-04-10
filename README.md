# E-voting-system-using-blockchain-and-python

## About Applicaiton
Let us briefly define the scope of our mini-application. The goal is to build an application that allows Voter to vote for the party they want with their Voter ID. One voter can only vote one time with their Unique Voter ID. Since the voted information will be stored on the blockchain, it'll be immutable and permanent. Users will interact with the application via a simple web interface.

We'll follow a bottom-up approach to implement things. Let's begin by defining the structure of the data that we'll store in the blockchain. Three essential elements will identify a post (message posted by any user on our application):

* party (Political parties, Leader)
* voter_id (Voter with their voter ID)
* Timestamp

We'll be storing data in our blockchain in a format that's widely used: JSON. Here's what a post stored in blockchain will look like:

```
"transactions": [
        {
          "voter_id": "VOID001",
          "party": "Democratic Party",
          "timestamp": 1649571086.02753
        }
      ],
```

The transactions are packed into blocks. A block can contain one or many transactions. The blocks containing the transactions are generated frequently and added to the blockchain. Because there can be multiple blocks, each block should have a unique id:
```
class Block:
    def __init__(self, index, transactions, timestamp, previous_hash, nonce=0):
        self.index = index
        self.transactions = transactions
        self.timestamp = timestamp
        self.previous_hash = previous_hash
        self.nonce = nonce
```

## What is blockchain? How it is implemented? And how it works?

Blockchain is a technology that is rapidly gaining momentum in current era. With high security and transparency provisions, it is being widely used in supply chain management systems, healthcare, payments, business, IoT, voting systems, etc.

Blockchain is a way of storing digital data. The data can literally be anything. For Bitcoin, it's the transactions (logs of transfers of Bitcoin from one account to another account), but it can even be files; it doesn't matter. The data is stored in the form of blocks, which are linked (or chained) together by cryptographic hashes. Hence the name "blockchain."

All of the magic lies in the way this data is stored and added to the blockchain. A blockchain is essentially a linked-list containing ordered-data, with some constraints like below;

* Blocks can't be modified once added; in other words, it is "append-only."
* There are specific rules for appending data to it.
* It's distributed in architecture.
* Enforcing these constraints yields some highly desirable characteristics:

* Immutability and durability of data
* No single point of control or failure
* A verifiable audit trail of the order in which data was added

### Why we need Blockchain based e-voting system
Current voting systems like ballot box voting or electronic voting suffer from various security threats such as DDoS attacks, polling booth capturing, vote alteration and manipulation, malware attacks, etc, and also require huge amounts of paperwork, human resources, and time. This creates a sense of distrust among existing systems.

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

## References
1. https://www.geeksforgeeks.org/decentralized-voting-system-using-blockchain/

