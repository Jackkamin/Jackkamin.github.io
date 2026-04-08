---
layout: post
title: "Springboot & Database updates"
date: 2026-04-08
---

I have begun a personal finance project, the idea is to have all my outgoings and incomings on the app.
I have implemented a Springboot backend using Kotlin. I ran into some problems with folder structure however with help from Claude

DISCLAIMER**
majority of this setup was from claude, i just altered it and tried my best to ask the right questions.
## MAIN APPLICATION
```kotlin
package com.jackkamin.finex

import org.springframework.boot.autoconfigure.SpringBootApplication
import org.springframework.boot.runApplication

@SpringBootApplication
class FinExApplication

fun main(args: Array<String>) {
    runApplication<FinExApplication>(*args)

//This is the main kt file, this is what we build.
}
```
## TRANSACTION MODEL 
Every time someone POSTs a transaction, it gets stored as one of these. It tells the database "a transaction has these fields: an ID, an amount, a description, a category, a type, and a date.
@Entity lets us tell the database to create a table for it.
```kotlin
package com.jackkamin.finex

import jakarta.persistence.*
import java.time.LocalDate

@Entity
data class Transaction(
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    val id: Long = 0,
    val amount: Double,
    val description: String,
    val category: String,
    val type: String,
    val date: LocalDate = LocalDate.now()
)
//Controls what the transaction holds, ex: date, amount, category
// Defines the structure of a Transaction. Each field maps to a column in the TRANSACTION table in the database.
```
## TRANSACTION CONTROLLER
The controller handles incoming HTTP requests. It gives us three endpoints — GET, POST and DELETE.
```kotlin
package com.jackkamin.finex

import org.springframework.web.bind.annotation.*

@RestController
@RequestMapping("/transactions")
class TransactionController(private val repository: TransactionRepository) {

    @GetMapping("/{id}")
    fun getById(@PathVariable id: Long): Transaction {
        return repository.findById(id).orElseThrow()
    }

    @PostMapping
    fun create(@RequestBody transaction: Transaction): Transaction {
        return repository.save(transaction)
    }

    @DeleteMapping("/{id}")
    fun delete(@PathVariable id: Long) {
        repository.deleteById(id)
    }
}
```
## REPOSITORY
Our connection to the database, handles SQL for us.
```kotlin
//Database repo

package com.jackkamin.finex
import org.springframework.data.jpa.repository.JpaRepository

interface TransactionRepository : JpaRepository<Transaction, Long>

//Connection to Database http://localhost:8080/h2-console/
```
