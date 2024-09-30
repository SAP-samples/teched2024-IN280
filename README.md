# IN280 - Use SAP Integration Suite with SAP IBP and third-party systems

## Description

This repository contains the material for the SAP TechEd 2024 session IN280 - Use SAP Integration Suite with SAP IBP and third-party systems.  

## Overview

This session introduces attendees to Non-SAP integration of SAP IBP through SAP Integration Suite. In this session we will be using Amazon AWS S3 and Snowflake as non-SAP cloud services.

<br>![](/exercises/ex0/images/00_00_0010.png)

## Requirements

The requirements to follow the exercises in this repository are:


- [Getting Started](exercises/ex0/)

## Exercises

The integration between SAP IBP and 3rd party systems such as Amazon S3 or Snowflake uses SAP Cloud Integration as a widdleware. The below steps outline a basic setup which can be excercised to exchange data between SAP IBP and Snowflake using Amazon S3 as a staging section. Participants are requested to read thru the steps and follow them on their own SAP cloud instances. 

- [Getting Started](exercises/ex0/)
- [Exercise 1 - Environment Setup - 1](exercises/ex1/)
    - [Exercise 1.1 - Communication Arrangement for communication scenario - SAP_COM_0931 in SAP IBP](exercises/ex1#exercise-11-sub-exercise-1-description)
    - [Exercise 1.2 - RFC Destination in SAP BTP Cockpit](exercises/ex1#exercise-12-sub-exercise-2-description)
    - [Exercise 1.3 - Copy and Deploy SAP Cloud Integration Standard package "SAP IBP - Reusable Integration Flows"](exercises/ex1#exercise-13-sub-exercise-3-description)
    - [Exercise 1.4 - Copy and Deploy SAP Cloud Integration Community package "Session IN280"](exercises/ex1#exercise-14-sub-exercise-4-description)
- [Exercise 2 - Environment Setup - 2](exercises/ex2/)
    - [Exercise 2.1 - Create table in Snowflake](exercises/ex2#exercise-21-sub-exercise-1-description)
    - [Exercise 2.2 - Create external Stage using AWS S3](exercises/ex2#exercise-22-sub-exercise-2-description) 
- [Exercise 3 - Reading from SAP IBP and Writing to Snowflake](exercises/ex3/)
    - [Exercise 3.1 - Overview](exercises/ex3#Overview)
    - [Exercise 3.2 - Input parameters](exercises/ex3#Input)
    - [Exercise 3.3 - Mapping](exercises/ex3#Mapping)
    - [Exercise 3.4 - Staging](exercises/ex3#Staging)
    - [Exercise 3.5 - Copying](exercises/ex3#Copying)
- [Exercise 4 - Reading from Snowflake and Writing to SAP IBP](exercises/ex4/)
    - [Exercise 4.1 - Overview](exercises/ex4#Overview)
    - [Exercise 4.2 - Input parameters](exercises/ex4#Input)
    - [Exercise 4.3 - Mapping](exercises/ex4#Mapping)
    - [Exercise 4.4 - IBP Processes](exercises/ex4#IBP-Processes)
- [Exercise 5 - Conclusion](exercises/ex5/) 

**IMPORTANT**

These steps are intended to be a guide for learning purposes only. The setup might change based on your instance configuration or IT landscape.  

## Contributing
Please read the [CONTRIBUTING.md](./CONTRIBUTING.md) to understand the contribution guidelines.

## Code of Conduct
Please read the [SAP Open Source Code of Conduct](https://github.com/SAP-samples/.github/blob/main/CODE_OF_CONDUCT.md).

## How to obtain support

Support for the content in this repository is available during the actual time of the online session for which this content has been designed. Otherwise, you may request support via the [Issues](../../issues) tab.

## License
Copyright (c) 2024 SAP SE or an SAP affiliate company. All rights reserved. This project is licensed under the Apache Software License, version 2.0 except as noted otherwise in the [LICENSE](LICENSES/Apache-2.0.txt) file.
