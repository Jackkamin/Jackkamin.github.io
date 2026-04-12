---
layout: post
title: "PUT it down!"
date: 2026-04-12
---

So i added a PUT endpoint for our database, seems to work fine.

```kotlin
 @PutMapping("/{id}")
    fun update(@PathVariable id: Long, @Valid @RequestBody transaction: Transaction): Transaction {
        val existing = repository.findById(id).orElseThrow()
        val updating = transaction.copy(
            amount = transaction.amount,
            description = transaction.description,
            category = transaction.category,
            type = transaction.type,
            date = transaction.date,
            id = existing.id
        )
        return repository.save(updating)
```

I am going to work on my Kotlin some more as i feel i am relying alot on Claude.
