# chainpoint

[![Build Status](https://travis-ci.org/Storj/hashserv.svg?branch=master)]
(https://travis-ci.org/Storj/hashserv) 
[![Coverage Status](https://coveralls.io/repos/Storj/hashserv/badge.svg?branch=master)](https://coveralls.io/r/Storj/hashserv?branch=master)

# What is this?

We want to prove some data existed at a certain point and time, we call this
[Proof of Existence](http://www.proofofexistence.com/about). We can do that by finding
the cryptographic hash of some data, then inserting that hash into the Bitcoin blockchain.
Since the Bitcoin blockchain is an immutable and secure public ledger, we can prove that 
the data existed at the time of insertion, like a mathematical notary. Unfortunately that
insertion costs money, around $0.03, so it becomes expensive if I wanted to do this for
10,000+ documents. 

Instead we can take that data and put it in a 
[Merkle tree](https://en.wikipedia.org/wiki/Merkle_tree), and insert the Merkle root into
the Bitcoin blockchain. For any of the items in the Merkle tree, as long as we have the Merkle proof,
we can prove that it is contained in that Merkle root notarized in the Bitcoin blockchain. 

Therefore we can notarize 10,000 documents for $0.03 instead of $300. It is 
recommended that the party notarizing their document or data hold on to the Merkle proof "receipt." 
This allows that party to prove that their document has been notarized in a specific Bitcoin transaction
in the absence of any other data.

**tldr;** A federated server for building blockchain notarized Merkle trees, and returning the Merkle
proof as a receipt. 

# API

### Submission
Add a hash to the queue for the next block. Returns the block number that the data will
be inserted into. In some cases the hash is a duplicate, so the API will return the block
number that the hash is in.
    
	GET /api/submit/<sha256_hash>/
        
Success Example:

	GET /api/submit/c059c8035bbd74aa81f4c787c39390b57b974ec9af25a7248c46a3ebfe0f9dc8/
	RESPONSE: 
	    Status Code: 200
	    Payload: 
	    	{	
	    		"hash": "c059c8035bbd74aa81f4c787c39390b57b974ec9af25a7248c46a3ebfe0f9dc8",
	    		"block": 5
	    	}
	    	
Fail Example:

	GET /api/submit/invalidhash/
	RESPONSE:
		Status Code: 400
		Payload:
			{
				"error": "Invalid hash.",
				"error-code": 400
			}
	
