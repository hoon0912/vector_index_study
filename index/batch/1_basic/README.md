
# How to start?
```shell
jupyter noteboook
# open basic_1.ipynb 
```

# How to check?
```shell
# open kibana dev tool (http://localhost:5601/app/dev_tools#/console)
# run below command
GET _search
{
  "query": {
    "match_all": {}
  }
}
```