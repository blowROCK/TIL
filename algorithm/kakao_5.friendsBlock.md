블라인드 공채를 통과한 신입 사원 라이언은 신규 게임 개발 업무를 맡게 되었다. 이번에 출시할 게임 제목은 “프렌즈4블록”.
같은 모양의 카카오프렌즈 블록이 2×2 형태로 4개가 붙어있을 경우 사라지면서 점수를 얻는 게임이다.

![](/img/pang1.png)


만약 판이 위와 같이 주어질 경우, 라이언이 2×2로 배치된 7개 블록과 콘이 2×2로 배치된 4개 블록이 지워진다. 
같은 블록은 여러 2×2에 포함될 수 있으며, 지워지는 조건에 만족하는 2×2 모양이 여러 개 있다면 한꺼번에 지워진다.

![](/img/pang2.png)

블록이 지워진 후에 위에 있는 블록이 아래로 떨어져 빈 공간을 채우게 된다.

![](/img/pang3.png)

만약 빈 공간을 채운 후에 다시 2×2 형태로 같은 모양의 블록이 모이면 다시 지워지고 떨어지고를 반복하게 된다.

![](/img/pang4.png)

위 초기 배치를 문자로 표시하면 아래와 같다.

```
TTTANT
RRFACC
RRRFCC
TRRRAA
TTMMMF
TMMTTJ
```

각 문자는 라이언(R), 무지(M), 어피치(A), 프로도(F), 네오(N), 튜브(T), 제이지(J), 콘(C)을 의미한다

입력으로 블록의 첫 배치가 주어졌을 때, 지워지는 블록은 모두 몇 개인지 판단하는 프로그램을 제작하라.

### 입력 형식

- 입력으로 판의 높이 m, 폭 n과 판의 배치 정보 board가 들어온다.
- 2 ≦ n, m ≦ 30
- board는 길이 n인 문자열 m개의 배열로 주어진다. 블록을 나타내는 문자는 대문자 A에서 Z가 사용된다.

### 출력 형식

입력으로 주어진 판 정보를 가지고 몇 개의 블록이 지워질지 출력하라.


### 입출력 예제
```
m 	n 	board 	answer
4 	5 	[“CCBDE”, “AAADE”, “AAABF”, “CCBBF”] 	14
6 	6 	[“TTTANT”, “RRFACC”, “RRRFCC”, “TRRRAA”, “TTMMMF”, “TMMTTJ”] 	15
```


```

var array = ["CCBDE", "AAADE", "AAABF", "CCBBF"];
var n = 4;
var m = 5;

var array = ["TTTANT", "RRFACC", "RRRFCC", "TRRRAA", "TTMMMF", "TMMTTJ"];
var n = 5;
var m = 6;


var copyBlockArray = [];
var detactChange = false;


String.prototype.replaceAt=function(index, character) {
    return this.substr(0, index) + character + this.substr(index+character.length);
}


function getTotalPoint(pointArray){
    var totalPoint = 0;
    for(var i=0; i<n; i++){
        for(var j=0; j<m; j++){
            if(pointArray[i][j] == '0'){
                totalPoint++;
            }
        }
    }
    return totalPoint;
}


function moveBottom(moveArray){
    for(var j=0; j<m; j++){
        var point = -1;
        for(var i=n-1; i>=0; i--){
            if(moveArray[i][j] == '0' && point == -1){
                point = i;
            }
            else if(point >= 0 && moveArray[i][j] != '0'){
                var moveChart = moveArray[i][j];
                moveArray[point] = moveArray[point].replaceAt(j, moveChart);
                moveArray[i] = moveArray[i].replaceAt(j, '0');
                point--;
            }
        }
    }
}


function copyArray(exportArray){
    var returnArray = []
    for(var i=0; i<exportArray.length; i++){
        returnArray.push(JSON.parse(JSON.stringify(exportArray[i])))
    }
    return returnArray;
}


function moveCheck(x, y){
    copyBlockArray[x] = copyBlockArray[x].replaceAt(y, '0');
    copyBlockArray[x+1] = copyBlockArray[x+1].replaceAt(y, '0');
    copyBlockArray[x] = copyBlockArray[x].replaceAt(y+1, '0');
    copyBlockArray[x+1] = copyBlockArray[x+1].replaceAt(y+1, '0');
}

function block(x, y){
    if(x+1 < n && y+1 < m){
        if(array[x][y] != '0' && array[x+1][y] != '0' && array[x][y+1] != '0' && array[x+1][y+1] != '0'){
            if(array[x][y] && array[x+1][y] && array[x][y+1] && array[x+1][y+1]){
                if(array[x][y] == array[x+1][y] &&  array[x+1][y] == array[x][y+1] && array[x][y+1] == array[x+1][y+1]){
                    return true;
                }
            }
        }
    }
    return false;
}

function detact(){
    var checkDetact = false;
    for(var i=0; i<n; i++){
        for(var j=0; j<m; j++){
            if(block(i, j) == true){
                checkDetact = true;
                moveCheck(i, j);
            }
        }
    }

    if(checkDetact == true){
        detactChange = true;
    }
    else{
        detactChange = false;
    }
}

function draw(blockArray){
    for(var i=0; i<n; i++){
        console.log(blockArray[i]);
    }
}

function run(){
    
    do{
        draw(array);
        copyBlockArray = copyArray(array);
        detact();
        array = copyArray(copyBlockArray);
        moveBottom(array);
        console.log("\n------------\n");
    }
    while(detactChange == true);

    console.log(getTotalPoint(array));
}

run();

```
