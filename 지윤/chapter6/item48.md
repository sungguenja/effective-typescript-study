#### 아이템48. API 주석에 TSDoc 사용하기

- TSDoc으로 주석을 달아두면, 함수를 사용하는 곳에서 각 매개변수와 관련된 설명을 볼 수 있다. (@param, @returns)
- TSDoc은 markdown 형식으로 꾸며지기에 **\*\*굵은 글씨\*\***, _*기울임글씨*_ 등을 사용할 수 있다.

```typescript
/** _이름_을 출력합니다
 * @params user 유저 객체
 */
function printUserName(user: User) {
  console.log(user.name);
}
```
