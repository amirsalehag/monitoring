# restore snapshot from one cluster to another steps

* Make a repository from `kibana > Stack Management > Snapshot and Restore` in its ui , register a repository , name it for example `place` , with `shared filesystem` repo type, name the file system location place , and then register it.

* Check the indices:
```
GET _cat/indices?v
```
* Create a snapshot from a specific index:
```
PUT /_snapshot/place/place2?wait_for_completion=true
{
  "indices": "place_2",
  "ignore_unavailable": true,
  "include_global_state": false
}
```
we name the snapshot as `place2` in the specified repository which its `place`.

* Copy the `place` repo's directory from its nodes elastic backup path (in our case, its elasticsearches backup nodes are mounted here: '178.38' /mnt/nfs/elastic/place) , and then put it in the other node where the other clusters are located.

* Make a repository with the same name and pattern as the first cluster, repo name `place` and location name `place` also , and then copy the place directories contents inside the newly created backup directory which should be also called `place` , and then again, go to the second clusters kibana ui , delete the repository that you just registered (it doesnt remove its data, dont worry) and then recreate it with the same pattern and it should show that theres a snapshot available inside that repo, a just beside the repo ui , click on snapshots and restore `place2` the name of the snapshot that has been taken from the first cluster, and it should be find.
