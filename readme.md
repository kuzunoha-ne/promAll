# QuickStart

```
docker-compose up -d
npm run build
npm run start
```
# Source

./src/app.ts

```typescript
import {MongoClient, MongoClientOptions} from "mongodb";

const uri:string = "mongodb://127.0.0.1:27017"
const options:MongoClientOptions = {useUnifiedTopology:true,useNewUrlParser:true}
const client = new MongoClient(uri,options);

const dbName = "__test__";
const collectionName = "test_collec";

const insertDatas:any[] = [
  {id:1,data:"hogehoge"},
  {id:2,data:"piyopiyo"},
  {id:3,data:"fugafuga"},
  {id:4,data:"maido"},
  {id:5,data:"ponta"},
  {id:6,data:"daikon"},
  {id:7,data:"tunacurry"},
  {id:8,data:"kutibiru"},
];

const initing = async (client:MongoClient) => {
  // MongoDB Connecting
  await client.connect();
  const __test__ = client.db(dbName);
 
  // Insert Any Datas
  await __test__.collection(collectionName).insertMany(insertDatas);

  // Create DB Connect
  const cursor = __test__.collection(collectionName).find();  

  // Find Any Datas
  /*   
  const datas = [];
  while(await cursor.hasNext()){
    datas.push(await cursor.next());
  };
  // Standard Output Datas
  console.log(datas);
  */
  const datas = [];
  const count = await cursor.count();
  for (let i = 1; i <= count; i++){
    datas.push(cursor.next());
  };
  const promised = await Promise.all(datas);
  // Standard Output Datas
  console.log(promised);

  // Close Connect and DropDatabase
  await cursor.close();
  await __test__.dropDatabase();
  await client.close();

  return ;
};

initing(client);
```
