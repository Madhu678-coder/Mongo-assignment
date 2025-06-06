﻿# Mongo-assignment

# MongoAssignment – MongoDB Todo Collection

This project demonstrates CRUD operations and schema validation on a `todos` collection within the `MongoAssignment` database using MongoDB.

## 📁 Database and Collection

- **Database:** `MongoAssignment`
- **Collection:** `todos`

---

## 📥 Insert Documents


db.todos.insertMany([
  { taskName: "Fix broken chair", status: false, priority: "Medium", description: "Repair the dining chair that has a loose leg." },
  { taskName: "Write a blog post", status: false, priority: "High", description: "Draft and publish a blog post on JavaScript best practices." },
  { taskName: "Organize photo album", status: false, priority: "Low", description: "Sort and arrange the digital photos by year and event." },
  { taskName: "Refill water bottles", status: false, priority: "Medium", description: "Clean and refill all reusable water bottles for the week." },
  { taskName: "Update antivirus software", status: false, priority: "Low", description: "Download and install the latest antivirus updates." },
  { taskName: "Apply for passport renewal", status: false, priority: "High", description: "Fill the online form and book an appointment at the passport office." },
  { taskName: "Clean refrigerator", status: false, priority: "Medium", description: "Remove expired items and wipe shelves." },
  { taskName: "Learn to bake bread", status: false, priority: "High", description: "Follow a YouTube tutorial and try baking a simple sourdough." },
  { taskName: "Visit local museum", status: false, priority: "Low", description: "Explore new exhibitions at the city’s heritage museum." },
  { taskName: "Renew Netflix subscription", status: false, priority: "Medium", description: "Ensure subscription is active for weekend shows." }
])
```

### ✅ Output

{
  acknowledged: true,
  insertedIds: {
    '0': ObjectId('6837e65721c4eddfc70c600c'),
    '1': ObjectId('6837e65721c4eddfc70c600d'),
    '2': ObjectId('6837e65721c4eddfc70c600e'),
    '3': ObjectId('6837e65721c4eddfc70c600f'),
    '4': ObjectId('6837e65721c4eddfc70c6010'),
    '5': ObjectId('6837e65721c4eddfc70c6011'),
    '6': ObjectId('6837e65721c4eddfc70c6012'),
    '7': ObjectId('6837e65721c4eddfc70c6013'),
    '8': ObjectId('6837e65721c4eddfc70c6014'),
    '9': ObjectId('6837e65721c4eddfc70c6015')
  }
}

## 🔍 Find Documents


db.todos.find()

### ✅ Output (Sample)
{
  _id: ObjectId('6837e65721c4eddfc70c600c'),
  taskName: 'Fix broken chair',
  status: false,
  priority: 'Medium',
  description: 'Repair the dining chair that has a loose leg.'
}

...

---

## 🔄 Update Operations

### Set `status` to `false` for All

db.todos.updateMany({}, { $set: { status: false } })

**Output:**

{ "acknowledged": true, "matchedCount": 10, "modifiedCount": 0 }

### Update Priorities


// High
db.todos.updateMany({ taskName: { $in: [ "Write a blog post", "Apply for passport renewal", "Learn to bake bread" ] } }, { $set: { priority: "High" } })
**Output:**
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 3,
  modifiedCount: 0,
  upsertedCount: 0
}

// Medium
db.todos.updateMany({ taskName: { $in: [ "Fix broken chair", "Refill water bottles", "Clean refrigerator", "Renew Netflix subscription" ] } }, { $set: { priority: "Medium" } })
**Output:**

  acknowledged: true,
  insertedId: null,
  matchedCount: 4,
  modifiedCount: 0,
  upsertedCount: 0
}
// Low
db.todos.updateMany({ taskName: { $in: [ "Organize photo album", "Update antivirus software", "Visit local museum" ] } }, { $set: { priority: "Low" } })

**Output:**
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 3,
  modifiedCount: 0,
  upsertedCount: 0
}

---

## 🛡️ Schema Validation


db.runCommand({
  collMod: "todos",
  validator: { priority: { $in: ["Low", "Medium", "High"] } },
  validationLevel: "moderate"
})


**Output:**

{ "ok": 1 }


### Attempt Invalid Insert

db.todos.insertOne({
  taskName: "Test invalid priority",
  status: false,
  priority: "Critical",
  description: "This should fail if schema validation is enforced."
})

**Output:**

MongoServerError: Document failed validation

---

## 📅 Date Fields

### Add `dueDate` and `updatedDate` (null initially)

db.todos.updateMany({}, { $set: { updatedDate: null, dueDate: null } })


### Set Dates for a Task

db.todos.updateOne(
  { taskName: "Fix broken chair" },
  { $set: { updatedDate: new Date(), dueDate: ISODate("2025-06-20T00:00:00Z") } }
)


### Output Example (as of 2025-05-29T04:59:44.188603 UTC)


{
  "taskName": "Fix broken chair",
  "status": false,
  "priority": "Medium",
  "dueDate": "2025-06-20T00:00:00Z",
  "updatedDate": "2025-05-29T04:47:12.708Z"
}


---

## 🗂️ Status Update by ID


db.todos.updateOne(
  { _id: ObjectId("6837e65721c4eddfc70c6012") },
  { $set: { status: true } }
)

**Output:**


{ "acknowledged": true, "matchedCount": 1, "modifiedCount": 1 }

---

