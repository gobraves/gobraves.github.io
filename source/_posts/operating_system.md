---
layout: post
title: "OS Note"
date: 2015-12-25
categories: 操作系统
---

## Operating System  

- virtualize CPU
  - time sharing
  - process
      - machine state
          - memory
          - registers
              - program counter
              - instruction pointer
              - frame pointer
              - stack pointer
      - process api
          - create
          - destroy
          - wait
          - miscellaneous control
          - status
  - question about process
      - how does os get a program up and running?
      - how does process creation actually work?
  - process states
      - Running
      - Ready
      - Blocked
  - mechanism
      - problem: how to perform restricted operations
      - solution: use protected control transfer
          - user mode
          - kernel mode
