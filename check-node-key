#!/usr/bin/python
import requests


SERVER = '192.168.56.22'
MASTER_KEY = "cluster_master"
MASTER_VALUE = "thekey"
MASTER_PAYLOAD = {MASTER_KEY:MASTER_VALUE}
NODES_ENDPOINT = "http://192.168.56.23:8500/v1/catalog/nodes"
KEY_ENDPOINT = "http://192.168.56.23:8500/v1/kv/"
allnodes = []


nodes_req = requests.get(NODES_ENDPOINT)

if nodes_req.status_code == 200:
	for items in nodes_req.json():
		if items['TaggedAddresses']['wan']!=SERVER:
			print items['TaggedAddresses']['wan']
			allnodes.append(items['TaggedAddresses']['wan'])
	if len(allnodes)==0:
		print "No nodes available"
		print "Bring up your cluster setup"
	elif len(allnodes)==1:
		print "Let's set up %s for cluster master" %allnodes[0]
                master_endpoint = KEY_ENDPOINT + MASTER_KEY
                master_req = requests.get(master_endpoint)
                if master_req.status_code == 404:
			print "Master Key Not found, let's set up cluster master"
			set_master = KEY_ENDPOINT + MASTER_KEY
			set_master_req = requests.put(set_master, data=MASTER_PAYLOAD)
			if set_master_req.status_code == 200:
				print "Master Key Loaded successfully"
			else:
				print "Check your configurations, Master Key Loading unsuccessful"
		elif master_req.status_code == 200:
			print "Master Key Found, please go ahead with setting up rest of the nodes for cluster"
	else:
		print "Your master should already be up, set the rest of the nodes"
