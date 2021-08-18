# User Story Practice: Retrieval of a Student's Grade
 Date: 2021.08.18 (W134)\
 Author: Carolyn Kao (chkaou@tsmc.com)\
Objective: To retrieve one's grade by functional domain design in TypeScript format. 

## User story

> **As a** student<br/>
**I want to** retrieve the grade from a specific class that I took<br/>
**So that** I can be informed of the grade

## Domain system design

A student should be verified to be a valid student. After the student is verified by providing his/her student id and student password, the grade-posting system would be able to show the grade for a specified class to the student. 

Authenticate a student:
```haskell
data LoginResult = LoginSuccess ClassIDList | LoginFailure Msg

authStudent :: Sid -> Pwd -> LoginResult
```
- Variable Type:
    * `Sid`: string
    * `Pwd`: string
    * `ClassIDList` (if success): Array\<string>
    * `Msg` (if failure): string


Get a grade from the system:
```haskell
data GetGradeResult = GetGradeSuccess Grade | GetGradeFailure Msg

getGrade :: ClassIDList -> ClassID -> GetGradeResult
```
- Variable Type:
    * `ClassIDList` (if success): Array\<string>
    * `ClassID`: string
    * `Grade` (if success): string
    * `Msg` (if failure): string

## Operations

### Test
> npm run test

In this unit test, as a student of student id `06366163` and password of `CarolynKao168`, I would like to access my grade for class `CS224N`. The resulting grade should match the grade of `A+`. 
```haskell
test('Test getting grade', () => {

    pipe(
        getGradeWorkflow(
            SidOf('06366163'))(
                PwdOf('CarolynKao168'))(
                    classIDOf('CS224N')),
                    M.match<GetGradeResult, void>({
                        Success: ({ grade: g }) => expect(g.grade).toBe('A+'),
                        Failure: ({ msg }) => fail(msg),
                        _: fail()
                    })
    )
})
```
