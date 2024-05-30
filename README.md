# Description

Here, In this template, we will deploy our first job using the script provided by HashiCorp into our cluster. 

Get up and running with Nomad by learning about scheduling, setting up a cluster, and deploying an example application.

The application simulates employees working at a technology company. They come online, complete their tasks, and then log off.

---

#### Pre-requisite

* Docker 
* Nomad 

---

### Steps:-

1. Run the agent in the terminal using the commmand

```
sudo nomad agent -dev \
  -bind 0.0.0.0 \
  -network-interface='{{ GetDefaultInterfaces | attr "name" }}'
```

2. In a second terminal session, export the cluster address.

    `export NOMAD_ADDR=http://localhost:4646`

3. Check the status of the node : 
    `nomad node status` 


4. Use the command to run :

  a. `nomad job run pytechco-redis.nomad.hcl`

  b. `nomad job run pytechco-web.nomad.hcl` 

  c. Get the UI of the application 

    ```
    nomad node status -verbose \
    $(nomad job allocs pytechco-web | grep -i running | awk '{print $2}') | \
    grep -i ip-address | awk -F "=" '{print $2}' | xargs | \
    awk '{print "http://"$1":5000"}'
    ``` 

  d.`nomad job run pytechco-setup.nomad.hcl`

  e.`nomad job dispatch -meta budget="200" pytechco-setup`

  f.`nomad job run pytechco-employee.nomad.hcl`

  g.`nomad job stop -purge pytechco-employee`

  h.`nomad job dispatch -meta budget="500" pytechco-setup`

  i.`nomad job run pytechco-employee.nomad.hcl`

5. Clean up the cluster :

  a.`nomad job stop -purge pytechco-employee`

  b.`nomad job stop -purge pytechco-web`

  c.`nomad job stop -purge pytechco-redis`

  d.`nomad job stop -purge pytechco-setup`
  
---