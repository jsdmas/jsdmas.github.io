---
title: "React-hook-form 사용 예시"
categories:
  - React
tags:
  - React
last_modified_at: 2023-03-20
---
[React Hook Form](https://react-hook-form.com/)   

React Hook Form을 사용하기 전 코드
```jsx
import { useState } from react;
const toDoForm = () => {
  const [toDo, setToDo] = useState("");
  const onSubmit = (event: React.FormEvent<HTMLFormElement>) => {
    event.preventDefault();
    console.log(toDo);
  };
  const onChange = (event: React.FormEvent<HTMLInputElement>) =>{
    const {currentTarget : {value} } = event;
    setToDo(value);
  };
return(
  <>
    <form onSubmit={onSubmit}>
      <input onChange={onChange} value={toDo} placeholder="to do..."/>
      <button>Add</button>
    </form>
  </>
)
};
export default toDoForm;
```

use React Hook Form 

```jsx
import { useForm } from "react-hook-form";
const toDoForm = () => {
  const { register } = useForm();
  <form>
    <input {...register("email")} placeholder="email"/>
    <input {...register("id")} placeholder="id"/>
    <input {...register("password")} placeholder="password"/>
    <input {...register("username")} placeholder="username"/>
    <button>Add</button>
  </form>
}
```

register 함수 내부  
![](https://user-images.githubusercontent.com/105098581/226515418-f39e3b03-6f1c-47b7-a9a7-e54a54fad3b3.png)  

useState를 남발하지 않을 수 있고 보기도 편하다.  

# validation 예시

```jsx
import { useForm } from "react-hook-form";

interface IForm {
  email : string
  firstName: string;
  lastName: string;
  username: string;
  password: string;
  password1: string;
  extraError?: string;
}

const toDoList = () => {
  const { register, handleSubmit, formState : {errors}, setError } = useForm<IForm>();
  const onVaild = (data:IForm) => {
    if(password !== password1){
      setError("password1", { message : "비밀번호가 같지 않습니다." }, { shouldFocus : true })
    }
  };
  return(
    <>
      <form onSubmit={handleSubmit(onVaild)}>
        // required : input에 required 항목을 적지 않을시 보여주는 에러 메세지.
        // required : "stinrg" -> required : true 와 같다.
        <input {...register("email", { 
          required: "Email is required",
          minLength : {
            value : 5,
            message : "5자보다는 길어야합니다."
          },
          pattern : {
            value : /^[A-Za-z0-9._%+-]+@gmail.com$/,
            message : "gamil만 가능합니다."
          }
        })} placeholder="Email" />
        <span>{errors?.email?.message}</span>
        <input {...register("firstName", {
          required: "firstName is required",
          // validate는 에러와 에러메세지를 커스텀해서 사용할 수 있습니다.
          validate : {
            noHello : (value) => value.includes("hello") ? "hello 는 포함할 수 없습니다." : true,
            noJinHo : (value) => value.includes("jinHo") ? "jinHo 는 포함훌 수 없습니다." : true,
          }
        })}/>
        <span>{errors?.firstName?.message}</span>
        <button>Add</button>
      </form>
    </>
  )
};
```
