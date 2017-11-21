## Adding data to firestore documents using Custom Class

1. Create the model class.
    ```java
    public class Student {
        private String name;
        private double cgpa;
        private boolean isCR;
        @ServerTimestamp private Date date;

        public Student(){
            this("zoha", 0.00, true);
        }

        public Student(String name, double cgpa, boolean isCR) {
            this.name = name;
            this.cgpa = cgpa;
            this.isCR = isCR;
        }

        public String getName() {
            return name;
        }

        public String getId() {
            return id;
        }

        public double getCgpa() {
            return cgpa;
        }

        public boolean isCR() {
            return isCR;
        }

        public Date getDate() {
            return date;
        }

        public void setDate(Date date) {
            this.date = date;
        }

        public static Map<String, Object> mapName(String name){
            Map<String, Object> data = new HashMap<>();
            data.put("name", name);

            return data;
        }
    }
    ```
    1.  Each custom class must have a public constructor that takes no arguments. In addition, the class must include a public getter for each property.
    1. The variable name will be used as the key for the value. you can change this behaviour and set different key using ```@PropertyName``` annotation. I have not used it yet.
    1. You can add ServerTimestamp by using ```@ServerTimestamp``` annotation in front of your __Date__ type variable. Don't forget to add setter along side getter for the variable. And you don't need to set the variable. Firestore will automatically set data to the variable.
    1. You can add some extra method that will return ```Map<S,T>``` type data. These methods would be useful for updating data.
    1. Available [annotation](http://googlecloudplatform.github.io/google-cloud-java/0.26.0/apidocs/com/google/cloud/firestore/annotation/package-summary.html) for Firestore model class.

1. Adding data using ```add()``` method.
    ```java
    FirebaseFirestore db = FirebaseFirestore.getInstance();

    Student student = new Student("kuddus",  3.88, false);

    db.collection("student").add(student)
        .addOnSuccessListener((DocumentReference documentReference)->{
            Log.d(TAG, "DocumentSnapshot added with ID: " + documentReference.getId());
        })
        .addOnFailureListener((@NonNull Exception e)->{
            Log.w(TAG, "Error adding document", e);
        });
    ```
    1. If you add data using ```add()``` method, Firestore will generate a unique id for you. you can get the id using ```documentReference``` variable. Use this method when you want to generate unique id for the documents.
    1. the return type of ```add()``` method is ```Task<DocumentReference>```

1. Adding data using ```set()``` method.
    ```java
    FirebaseFirestore db = FirebaseFirestore.getInstance();

    Student student = new Student("kuddus",  3.88, false);
    db.collection("student").document("std1").set(student)
        .addOnSuccessListener((Void aVoid)->{
            Log.d(TAG, "data has been set successfully");
        })
        .addOnFailureListener((@NonNull Exception e)->{
            Log.w(TAG, "Error adding document", e);
        });
    ```
    1. When you use ```set()``` to create a document, you must specify an ID for the document to create.
    1. If the document does not exist, it will be created. If the document does exist, its contents will be overwritten with the newly provided data, unless you specify that the data should be merged into the existing document, as follows:
        ```java
        db.collection("student").document("std1")
                .set(student, SetOptions.merge());
        ```
    1. The return type or ```set()``` method is ```Task<Void>```

## Updating data to firestore documents using Custom Class

1. You can easily update any data using a method from the model class that has ```Map<S,T>``` return type.
    ```java
    db.collection("student").document("std1").update(Student.mapName("boyati"))
            .addOnSuccessListener((Void aVoid)->{
                Log.d(TAG, "data has been set successfully");
            })
            .addOnFailureListener((@NonNull Exception e)->{
                Log.w(TAG, "Error adding document", e);
            });
    ```

    * The return type or ```update()``` method is ```Task<Void>```

## Querying data to firestore documents using Custom Class

1. [Perform Simple and Compound Queries in Cloud Firestore](https://firebase.google.com/docs/firestore/query-data/queries)
1. [Query reference (CollectionReference class extends Query)](https://firebase.google.com/docs/reference/android/com/google/firebase/firestore/Query)
1. At first create a Query object as per your need. Now call the ```get()``` method to execute the query and get the resultset. The return type of ```get()``` method is ```Task<QuerySnapshot>```
1. you can also get all the resultset from ```CollectionReference``` and  ```DocumentReference``` class by calling theri ```get()``` method.












## Links
1. [Add Data to Cloud Firestore](https://firebase.google.com/docs/firestore/manage-data/add-data)
1. [FirebaseUI for Cloud Firestore](https://github.com/firebase/FirebaseUI-Android/blob/master/firestore/README.md)
