# spring-boot-transaction
Java spring boot transaction

## Spring boot transaction propagation
---
![spring-boot-transaction](https://github.com/chowdhury18/spring-boot-transaction/blob/master/Diagrams/spring-transaction.png)
### ***Propagation.REQUIRED***:
```java
@Transaction(propagation=Propagation.REQUIRED)
insertTeam()
```
If the `insertTeam()` method is called directly, a new transaction will be created. Tag: `CREATE-NEW`

```java
@Transaction(propagation=Propagation.REQUIRED)
joinLeague() {
    insertTeam()
}

@Transaction(propagation=Propagation.REQUIRED)
insertTeam()
```
If the `insertTeam` method is called from `joinLeague` method:
- if the calling method has a transaction, the callee method propagates the existing transaction.
- if the calling method does not have a transaction, the callee method will create a new transaction.
- Tag: `CONTINUE` or `CREATE-NEW`

---
### ***Propagation.SUPPORTS***:
```java
@Transaction(propagation=Propagation.SUPPORTS)
insertTeam()
```
If the `insertTeam()` method is called directly, a new transaction will not be created. Tag: `NOT CREATED`

```java
@Transaction(propagation=Propagation.SUPPORTS)
joinLeague() {
    insertTeam()
}

@Transaction(propagation=Propagation.SUPPORTS)
insertTeam()
```
If the `insertTeam` method is called from `joinLeague` method:
- if the calling method has a transaction, the callee method propagates the existing transaction.
- if the calling method does not have a transaction, the callee method will not create a new transaction.
- Tag: `CONTINUE` and `NOT-CREATED`

---

### ***Propagation.NOT_SUPPORTED***:
```java
@Transaction(propagation=Propagation.NOT_SUPPORTED)
insertTeam()
```
If the `insertTeam()` method is called directly, a new transaction will not be created. Tag: `NOT CREATED`

```java
@Transaction(propagation=Propagation.NOT_SUPPORTED)
joinLeague() {
    insertTeam()
}

@Transaction(propagation=Propagation.NOT_SUPPORTED)
insertTeam()
```
If the `insertTeam` method is called from `joinLeague` method:
- if the calling method has a transaction, the callee method does not propagate the existing transaction nor create new.
- if the calling method does not have a transaction, the callee method will not create a new transaction.
- Tag: `NOT-CONTINUED` and `NOT-CREATED` -> `WITHOUT-TRANSACTION`

---

### ***Propagation.REQUIRES_NEW***:
```java
@Transaction(propagation=Propagation.REQUIRES_NEW)
insertTeam()
```
If the `insertTeam()` method is called directly, a new transaction will be created. Tag: `CREATE-NEW`

```java
@Transaction(propagation=Propagation.REQUIRES_NEW)
joinLeague() {
    insertTeam()
}

@Transaction(propagation=Propagation.REQUIRES_NEW)
insertTeam()
```
If the `insertTeam` method is called from `joinLeague` method:
- if the calling method has a transaction, the callee method does not propagate the existing transaction but create a new transaction.
- if the calling method does not have a transaction, the callee method will create a new transaction.
- Tag: `NOT-CONTINUED` but `CREATE-NEW`

---

### ***Propagation.NEVER***:
```java
@Transaction(propagation=Propagation.NEVER)
insertTeam()
```
If the `insertTeam()` method is called directly, a new transaction will not be created. Tag: `NOT-CREATED`

```java
@Transaction(propagation=Propagation.NEVER)
joinLeague() {
    insertTeam()
}

@Transaction(propagation=Propagation.NEVER)
insertTeam()
```
If the `insertTeam` method is called from `joinLeague` method:
- if the calling method has a transaction, the callee method does not propagate the existing transaction rather `throw` an exception.
- if the calling method does not have a transaction, the callee method will not create a new transaction and run without transaction.
- Tag: `NOT-CONTINUED` rather `THROW-EXCEPTION` and `NOT-CREATED` -> `WITHOUT-TRANSACTION`