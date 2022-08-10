## Qiwi's Documentation

### Usage (Example)

```
import gzip
import json
import urllib.request
import time
import numpy as np
from qiskit import QuantumCircuit
n=50;
circ = QuantumCircuit(n)
circ.h(0)
for ii in range(n-1):
  circ.cx(0, ii+1)
API_KEY = " "
API_ENDPOINT = "https://acceba8a34a7d49bfb20c708e4d7bb12-b00f58c1565b5c4d.elb.us-east-2.amazonaws.com/v2/"
import ssl
ctx = ssl.create_default_context()
ctx.check_hostname = False
ctx.verify_mode = ssl.CERT_NONE
def post_request(path, body):
  headers = {
    "Content-Type": "application/json",
    "X-Api-Key": API_KEY,
    "Accept-Encoding": "gzip",
  }
  req = urllib.request.Request(
    API_ENDPOINT + path, json.dumps(body).encode(), headers
  )
  with urllib.request.urlopen(req, context=ctx) as res:
    body = (
      gzip.decompress(res.read())
      if res.headers.get("Content-Encoding") == "gzip"
      else res.read()
    )
  return json.loads(body)
params = {
  "circuit": circ.qasm(),
  "num_samples": 100,
  "gate_batching": 50,
  "algo": "qr_iteration",
  "cf": 1e-14,
  "chi": 2,
}
# create task
res = post_request("quantum-tasks/create_itensors_tasks", params)
# get task by ID
time.sleep(30)
A = post_request("quantum-tasks/get_itensors_task", {'task_id': res["task_id"]})

```

You can get the above API key by registering an account on blueqat.com and clicking on the API key option from the drop down menu.


