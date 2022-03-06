# Getting started
```bash
docker-compose up -d
```
# Quit and remove volume
```bash
docker-compose down  -v
```
# Use mongocli
```bash
mongo
show dbs
db
use fruits
db
db.apples.insert(
{name: 'Red Delicious'}
)
show collections
help
db.apples.help()
db.a
db.apples.find()
```
