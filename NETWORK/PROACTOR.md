# Proactor 
원문 : http://www.cs.wustl.edu/~schmidt/PDF/proactor.pdf
 
  
An Object Behavioral Pattern for Demultiplexing
and Dispatching Handlers for Asynchronous Events

Irfan Pyarali, Tim Harrison, and Douglas C. Schmidt Thomas D. Jordan

This paper appeared at the 4th annual Pattern Languages
of Programming conference held in Allerton Park, Illinois,
September, 1997.

## Abstract

## 1 Intent

## 2 Motivation

### 2.1 Context and Forces

### 2.2 Common Traps and Pitfalls of Conventional
Concurrency Models

#### 2.2.1 Concurrency Through Multiple Synchronous hreads

#### 2.2.2 Concurrency Through Reactive Synchronous vent Dispatching

### 2.3 Solution: Concurrency Through Proactive Operations

## 3 Applicability


## 4 Structure and Participants

## 5 Collaborations

## 6 Consequences

### 6.1 Benefits

### 6.2 Drawbacks

## 7 Implementation

### 7.1 Implementing the Asynchronous Operation rocessor

#### 7.1.1 Define Asynchronous Operation APIs

#### 7.1.2 Implement the Asynchronous Operation Engine

### 7.2 Implementing the Completion Dispatcher

#### 7.2.1 Implementing Callbacks

#### 7.2.2 Defining Completion Dispatcher Concurrency Strategies

### 7.3 Implementing Completion Handlers

#### 7.3.1 State Integrity

#### 7.3.2 Resource Management

#### 7.3.3 Preemptive Policy

## 8 Sample Code

## 9 Known Uses

## 10 Related Patterns

## 11 Concluding Remarks

## References

## A Alternative Implementations

### A.1 Multiple Synchronous Threads

### A.2 Single-threaded Reactive Event Dispatching

