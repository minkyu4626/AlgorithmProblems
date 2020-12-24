## 완주하지 못한 선수
- 유형 : hash table 문제
- 문제 설명 : <https://programmers.co.kr/learn/courses/30/lessons/42576#>
- 출처 : <https://programmers.co.kr/learn/challenges>
- 날짜 : 2020.12.21

- 코드
```cpp
/*
 * 작성자 : minkyu4626@naver.com
 * 작성일 : 2020.12.18
 */

#include <string>
#include <vector>

using namespace std;

/*
 * parameter : string
 * 하는 일 : 문자열을 해쉬코드로, 해쉬코드를 해쉬테이블의 인덱스로
            변환하여 return하는 function
 * return : hash table의 index
 */
int Index(string key) {
    int hashcode = 0;
    for (int i = 0; i < key.length(); i++) {
        hashcode += (int)key[i];
    }
    int index = hashcode % 100000;
    return index;
}


vector<vector<string>> hashtable(99999); // 해쉬 테이블(완주한 선수는 최대 100,000-1명)
string solution(vector<string> participant, vector<string> completion) {
    string answer = "";
    int index;
    
    // 테이블에 마라톤 완주자의 정보 입력
    for (int i = 0; i < completion.size(); i++) {
        string str = completion[i];
        index = Index(str);
        hashtable[index].push_back(str);
    }
    
    // 마라톤 참가자들의 이름들이 테이블에 있는지 확인
    for (int i = 0; i < participant.size(); i++) {
        string str = participant[i];
        index = Index(str);
        if (hashtable[index].empty()) { // 해당 위치가 empty이면 완주하지 못한 선수이다.
            answer = str;
            break;
        }
        
        else { // not empty case
            int k;
            int size = hashtable[index].size();
            for (k = 0; k < size; k++) {
                if (hashtable[index][k] == str) { // hashtable[index]에 이름이 있으면 완주한 선수이다.
                    hashtable[index].erase(hashtable[index].begin()+k);
                    break;
                }
            }
            if (k == size) { // hashtable[index]에 이름이 없다면 완주하지 못한 선수이다.
                answer = str;
                break;
            }
        }
    }
    
    return answer;
}
```
- self feedback
> > 1. 단지 이 문제를 해결하기 위해서 내가 짠 알고리즘은 문제가 없다. 문제에서는 최대 참가자의 수가 100,000명(완주자는 99,999명)이라는 조건이 부여됬다.
   하지만 조건이 없는 경우에 참가자의 수가 적은 케이스를 보면 메모리상의 공간을 많이 낭비하게 된다. 예를 들어 참가자의 수가 3명인 케이스에는 최대 3개의 인덱스가
   해쉬 테이블에서 사용 된다. 따라서 낭비되는 인덱스는 100,000 - 3 = 99,997개가 사용되지 않고 낭비되고 있다. 그렇기 때문에 좀더 최적화를 한다면 각 케이스에서
   참가자의 수를 확인하여 해쉬 테이블의 크기를 알맞게 정한다면 낭비되는 공간을 줄일 수 있을 것 같다.
> > 2.  문제 해결을 위해서 충돌(collison) 문제를 해결하기 위해 각 인덱스에 list를 사용하여 구현하였다.
  하지만 이 때문에 다른 문제점 또한 생길 수 있다. 참가자들 중에서 이름은 다르지만 이름에 있는 알파벳들이 같은 사람들이 많은 경우 ex) jordan, jodarn...
  에는 같은 인덱스를 여러 사람의 이름들이 공유하게 된다.
  참가자의 수가 n이라고 하면 worst case의 경우 time complexity가 O(n)가 된다. 이렇게 되면 시간 효율성이 떨어지게 된다.
  사람의 이름이 이런 bad case를 형성하기는 어렵기 때문에 내가 만든 해쉬 알고리즘으로는 큰 문제가 생기지 않을 것이다.
  하지만 사람의 이름이 아닌 다른 문자열이라면? 효율적인 searching을 위해서는 알고리즘을 무조건 수정해서 최적화해야 할 것이다.

