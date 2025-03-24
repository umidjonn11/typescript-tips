# NestJS Methods Guide

## 1. Introduction
NestJS is a progressive Node.js framework for building efficient, scalable, and modular server-side applications using TypeScript. It is heavily inspired by Angular and follows the modular architecture.

---

## 2. Core Concepts & Methods

### 2.1 Modules
Modules are the building blocks of a NestJS application. Each module is a class decorated with `@Module()`. Modules help in organizing the application by grouping related components together. A module typically includes controllers, services, and providers.

#### Example:

```typescript
import { Module } from '@nestjs/common';
import { UsersController } from './users.controller';
import { UsersService } from './users.service';

@Module({
  controllers: [UsersController],
  providers: [UsersService],
})
export class UsersModule {}
```

This module registers the `UsersController` and `UsersService`, ensuring they are available within the application.

---

### 2.2 Controllers
Controllers handle incoming HTTP requests and return responses. They define route handlers and are decorated with `@Controller()`.

#### Example:

```typescript
import { Controller, Get, Post, Param, Body } from '@nestjs/common';
import { UsersService } from './users.service';

@Controller('users')
export class UsersController {
  constructor(private readonly usersService: UsersService) {}

  @Get()
  findAll() {
    return this.usersService.getAllUsers();
  }

  @Get(':id')
  findOne(@Param('id') id: number) {
    return this.usersService.getUserById(id);
  }

  @Post()
  create(@Body() userDto: { name: string; age: number }) {
    return this.usersService.createUser(userDto);
  }
}
```

Here, `@Get()` and `@Post()` decorators define endpoints for retrieving and creating users.

---

### 2.3 Services
Services contain business logic and interact with the database or other APIs. They are marked with `@Injectable()`.

#### Example:

```typescript
import { Injectable } from '@nestjs/common';

@Injectable()
export class UsersService {
  private users = [
    { id: 1, name: 'John Doe', age: 25 },
    { id: 2, name: 'Jane Smith', age: 30 }
  ];

  getAllUsers() {
    return this.users;
  }

  getUserById(id: number) {
    return this.users.find(user => user.id === id);
  }

  createUser(userDto: { name: string; age: number }) {
    const newUser = { id: Date.now(), ...userDto };
    this.users.push(newUser);
    return newUser;
  }
}
```

This service provides methods for retrieving and creating users, acting as the core logic layer.

---

### 2.4 Middleware
Middleware functions run before a request reaches the controller. They can be used for logging, authentication, and request modification.

#### Example:

```typescript
import { Injectable, NestMiddleware } from '@nestjs/common';
import { Request, Response, NextFunction } from 'express';

@Injectable()
export class LoggerMiddleware implements NestMiddleware {
  use(req: Request, res: Response, next: NextFunction) {
    console.log(`Request: ${req.method} ${req.url}`);
    next();
  }
}
```

To apply the middleware:

```typescript
import { Module, MiddlewareConsumer, NestModule } from '@nestjs/common';
import { UsersController } from './users.controller';
import { LoggerMiddleware } from './logger.middleware';

@Module({
  controllers: [UsersController],
})
export class AppModule implements NestModule {
  configure(consumer: MiddlewareConsumer) {
    consumer.apply(LoggerMiddleware).forRoutes(UsersController);
  }
}
```

This middleware logs each request before it reaches the controller.

---

### 2.5 Observables & RxJS
NestJS supports reactive programming using RxJS (Reactive Extensions for JavaScript). Observables are useful for handling asynchronous data streams efficiently.

#### Example:

```typescript
import { Injectable } from '@nestjs/common';
import { Observable, of } from 'rxjs';

@Injectable()
export class DataService {
  getData(): Observable<string> {
    return of('Hello from Observable!');
  }
}
```

To use this service in a controller:

```typescript
import { Controller, Get } from '@nestjs/common';
import { DataService } from './data.service';
import { Observable } from 'rxjs';

@Controller('data')
export class DataController {
  constructor(private readonly dataService: DataService) {}

  @Get()
  getData(): Observable<string> {
    return this.dataService.getData();
  }
}
```

Here, `getData()` returns an observable, and the response is streamed efficiently.

---

### 2.6 Exception Filters
Exception filters catch and handle errors globally.

#### Example:

```typescript
import { Catch, ExceptionFilter, ArgumentsHost, HttpException } from '@nestjs/common';

@Catch(HttpException)
export class HttpExceptionFilter implements ExceptionFilter {
  catch(exception: HttpException, host: ArgumentsHost) {
    const ctx = host.switchToHttp();
    const response = ctx.getResponse();
    const status = exception.getStatus();
    response.status(status).json({ error: exception.message });
  }
}
```

To apply it globally:

```typescript
import { Module } from '@nestjs/common';
import { APP_FILTER } from '@nestjs/core';
import { HttpExceptionFilter } from './http-exception.filter';

@Module({
  providers: [{ provide: APP_FILTER, useClass: HttpExceptionFilter }],
})
export class AppModule {}
```

This filter intercepts `HttpException` and customizes the error response.

---

## 3. Database & Repositories (TypeORM Example)

### 3.1 Entity Definition

```typescript
import { Entity, Column, PrimaryGeneratedColumn } from 'typeorm';

@Entity()
export class User {
  @PrimaryGeneratedColumn()
  id: number;

  @Column()
  name: string;

  @Column()
  age: number;
}
```

### 3.2 Repository Usage

```typescript
import { Injectable } from '@nestjs/common';
import { InjectRepository } from '@nestjs/typeorm';
import { Repository } from 'typeorm';
import { User } from './user.entity';

@Injectable()
export class UsersService {
  constructor(
    @InjectRepository(User)
    private usersRepository: Repository<User>,
  ) {}

  findAll() {
    return this.usersRepository.find();
  }

  findOne(id: number) {
    return this.usersRepository.findOne({ where: { id } });
  }

  async createUser(name: string, age: number) {
    const user = this.usersRepository.create({ name, age });
    return this.usersRepository.save(user);
  }
}
```

This repository allows easy interaction with the database.

---

## 4. Running the Application

```sh
npm install
npm run start:dev
```

This guide now provides detailed explanations and examples for a clearer understanding of NestJS.
