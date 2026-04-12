---
layout: post
title: "Validating"
date: 2026-04-12
---

Today i worked on validation so the system can funnel out bad data and us handle requests more efficiently.

I added checks to each value so it filters wrong info.
```kotlin
package com.jackkamin.finex

import jakarta.persistence.*
import java.time.LocalDate
import jakarta.validation.constraints.*

@Entity
data class Transaction(
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    val id: Long = 0,
    @field:Positive(message = "Amount must be positive")
    val amount: Double,
    @field:NotBlank(message = "Amount must not be blank")
    val description: String,
    @field:NotBlank(message = "Description must not be blank")
    val category: String,
    @field:NotBlank(message = "Category must not be blank")
    val type: String,
    val date: LocalDate = LocalDate.now()
)
```

My next task is adding a PUT endpoint so we can edit already existing data.
