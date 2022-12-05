# intro-jdbc

## Skapa projekt

```bash
cd ~
cd ws
rm -rf edu-intro-jdbc #Försiktig med denna
mkdir edu-intro-jdbc
cd edu-intro-jdbc
mkdir -p ./app/src/main/{java/se/iths,resources}
mkdir -p ./app/src/test/{java/se/iths,resources}
touch ./app/src/main/java/se/iths/App.java
touch ./app/src/test/java/se/iths/AppTest.java
touch ./app/build.gradle
echo "# edu-intro-java" > README.md
echo "rootProject.name = 'edu-intro-jdbc'\ninclude('app')" > settings.gradle
curl -L https://gist.github.com/miwashiab/987826fc0f2df3cd686a755f38a1c504/raw/build.gradle -o ./app/build.gradle
curl -L https://gist.github.com/miwashiab/0ca40c177e62925e8dbb973229a4299d/raw/AppTest.java -o ./app/src/test/java/se/iths/AppTest.java
curl -L https://gist.github.com/miwashiab/629757ac8e86e4caeab6835396be159b/raw/App.java -o ./app/src/main/java/se/iths/App.java
echo ".idea\n.gradle\nbuild\n*.log" > .gitignore
git init
git add .
git commit -m "Initial commit"
```
## AppTest.java

```bash
vi ./app/src/test/java/se/iths/AppTest.java
```

```java
package se.iths;

import org.junit.jupiter.api.Test;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;

class AppTest {
    @Test void firstDipShouldWork() {
        try {
            Connection con = DriverManager.getConnection("jdbc:mysql://localhost:3306/iths", "root", "root");
            ResultSet rs = con.createStatement().executeQuery("Show tables");
            while (rs.next()) {
                System.out.print(rs.getString(1));
                System.out.println();
            }
        } catch (Throwable e){
            System.out.println(e);
        }
    }
}
```

## Låna kod

```groovy
implementation group: 'mysql', name: 'mysql-connector-java', version: '8.0.30'
```
