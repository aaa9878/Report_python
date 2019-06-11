# 2019년도 1분기 부산도시철도 승하차 인원 분

학과 | 학번 | 성명
---- | ---- | ---- 
수학과 |201611124 |서진호


## 프로젝트 개요

1. 사용자가 입력한 정보를 저장할 리스트 info를 정의한다.

2. pandas 모듈으로 부터 공공데이터 '부산교통공사_시간대별_승하차인원_2019년_1분기.csv' 파일을 읽어서 data에 저장한다.

3. 조회하려는 호선을 Check()함수를 이용하여 입력한다. 단, 올바르지 않는 정보를 입력하면 오류 메시지를 출력한 후 계속해서 입력을 유도한다.
만약 전체 호선을 조회하려면 0을 입력한다.
이때, 입력값이 0이 아니라면 그 값을 info에 할당한다.
입력값이 0이라면 과정 6으로 넘어간다.

4. 조회하려는 역명을 Name_Check() 함수를 이용하여 입력한다. 단, 올바르지 않는 정보를 입력하면 오류 메시지를 출력한 후 계속해서 입력을 유도한다.
만약 해당 노선의 전체역을 조회하려면 0을 입력한다.
이때, 입력값이 0이 아니라면 그 값을 info에 할당한다.
입력값이 0이라면 과정 6으로 넘어간다.

5. 조회하려는 시간대를 Check() 함수를 이용하여 입력한다. 단, 올바르지 않는 정보를 입력하면 오류 메시지를 출력한 후 계속해서 입력을 유도한다.
만약 해당 역의 전체 시간대를 조회하려면 0을 입력한다.
이때, 입력값이 0이 아니라면 그 값을 info에 할당한다.

6. info에 저장된 결과를 바탕으로 계산된 결과값을 나타내는 DataFrame을 만들어서 이를 출력한다.

7. 그래프 출력 유형을 Check() 함수를 이용하여 입력한다.
만약 그래프 출력을 원치 않을 경우 0을 입력한다.

8. 과정 7에서 입력한 값이 0이 아닐경우, matplotlib 모듈을 통해 Print_graph() 함수를 이용하여 해당하는 자료의 그래프를 출력한다.


## 사용한 공공데이터 
[데이터보기](https://github.com/aaa9878/Report_python/blob/master/Download(s)/%EB%B6%80%EC%82%B0%EA%B5%90%ED%86%B5%EA%B3%B5%EC%82%AC_%EC%8B%9C%EA%B0%84%EB%8C%80%EB%B3%84_%EC%8A%B9%ED%95%98%EC%B0%A8%EC%9D%B8%EC%9B%90_2019%EB%85%84_1%EB%B6%84%EA%B8%B0.csv)

## 소스
* [링크로 소스 내용 보기](https://github.com/aaa9878/Report_python/blob/master/Download(s)/Report_python.py) 

* 코드 삽입
~~~python
import pandas as pd
from collections import OrderedDict as od
import warnings as w


#경고문 무시
w.filterwarnings(action='ignore')


# C언어의 switch문의 아이디어를 사용하여, 모든 역의 호선을 data에 할당하기 위한 함수
def My_switch(row) :
    return {0 : 1, 1 : 1, 2 : 2, 3 : 3, 4 : 4}.get(row['호선'], 0)



# 범위에 맞는 정수만 입력받아서 정수형을 리턴하는 함수
def Check(message, x, y) :

    warn = ""
    while True :
        user = input(warn + message)

        warn = "입력 오류!!\n다시 입력하세요.\n\n"

        if user.isdigit() :     # 실수로 숫자가 아닌 다른것을 입력하였을 경우
            user = int(user)

            if user in range(x, y) :
                break

    return user



# 올바른 역명만 입력받아서 그 정보를 리턴하는 함수
def Name_Check(message, num, name) :

    warn = ""
    while True :
        user = input(warn + message)
        warn = "입력 오류!!\n다시 입력하세요.\n\n"

        if user == "0" :
            user = int(user)
            break

        else :
            if user in ['서면', '동래', '덕천', '연산'] :   #환승역일 경우 구분하기 위함
                user = str(num) + user

            if user in name :
                break

    return user



# values.tolist()에 의해 만들어진 이차원 리스트의 모든 원소들의 평균을 구해서 결과리스트에 할당하는 함수
def Mean_values_tolist(dat_list, res_list) :

    tmp=[]
    for i in range(len(dat_list)) :
        for j in range(len(dat_list[i])) :
            tmp.append(int(str(dat_list[i][j]).replace(",", "")))

    res_list.append(round((sum(tmp)/len(tmp)), 2))

    return



# 일차원 리스트의 모든 원소들의 평균을 구해서 결과리스트에 할당하는 함수
def Mean_tolist(dat_list, res_list) :

    tmp = []
    for i in range(len(dat_list)) :
        tmp.append(int(str(dat_list[i]).replace(",", "")))

    res_list.append(round((sum(tmp)/len(tmp)), 2))

    return



# matplotlib 를 이용해서 출력하는 함수
def Print_graph(Ptype, Ires, Ores, Name, message) :
    Clist = range(len(Name))
    CIlist = [2*k for k in Clist]
    COlist = [2*k+1 for k in Clist]

    if Ptype == 3 :
        plt.bar(CIlist, Ires, label='승차', color='y')
        plt.bar(COlist, Ores, label='하차', color='m')
        plt.xticks(CIlist, Name)
    elif Ptype == 2 :
        Names = [x.replace("\n", "") for x in Name]
        plt.barh(CIlist, Ires, label='승차', color='y')
        plt.barh(COlist, Ores, label='하차', color='m')
        plt.yticks(CIlist, Names)

    elif Ptype == 1 :
        plt.plot(Clist, Ires, label='승차', c='y', lw=4, ls='--', marker='o')
        plt.plot(Clist, Ores, label='하차', c='m', lw=2, ls='--', marker='o')
        plt.xticks(Clist, Name)

    else :      # 예기치 못한 오류로 인해 Ptype에 이상한 값이 저장된 경우
        print("Unexpected Error!!\n")
        return


    plt.xlabel(message)
    plt.ylabel('평균(단위 : 명)')
    plt.title('부산도시철도 2019년도 1분기 승하차인원 평균 ')
    plt.legend()
    plt.show()

    return





if __name__ == "__main__" :

    info = []

    data = pd.read_csv("부산교통공사_시간대별_승하차인원_2019년_1분기.csv", encoding="cp949")
    data['호선'] = data['역번호']//100
    data['호선'] = data.apply(My_switch, axis=1)


    user1 = Check("조회하려는 노선을 입력하세요.\n단, 부산김해경전철과 동해남부선은 지원되지 않습니다.\n\n부산도시철도의 호선별 정보를 얻고 싶다면 0을 입력하세요.\n", 0, 5)

    if user1 != 0 :
        info.append(user1)

        name = data['역명'].tolist()
        Namedata = od.fromkeys(data.loc[data['호선']==user1]['역명'].tolist())

        user2 = Name_Check("조회하려는 역명을 입력하세요.\n※예시 : 부산대역을 조회하고 싶으면 부산대 입력\n\n단, 다른 호선으로 환승이 가능한 종착역(3호선의 수영역, 4호선의 미남역 등)은 지원되지 않으므로 다른 호선에서 선택해야 합니다.\n\n노선 전체에 대한 정보를 얻고 싶다면 0을 입력하세요.\n", user1, Namedata)

        if user2 != 0 :
            info.append(user2)
            user3 = Check("조회하려는 시간대를 입력하세요.\n단, 1시간 단위로만 조회가능하며, 입력시에 시작 시간을 입력하셔야 합니다.\n\n※예시1 : 오전 1시~ 오전 2시이면 1 입력\n※예시2 : 오후 4시~ 오후 5시 일 경우 16 입력\n※예시3 : 오후 11시~ 오전 12시 일 경우 23 입력\n※예시4 : 오전 12시~ 오전 1시 일 경우 24 입력\n\n전체 시간대에 대한 정보를 얻고 싶다면 0을 입력하세요.\n", 0, 25)

            if user3 != 0 :
                due = ''

                if user3 < 9 :
                    due = '0' + str(user3) + '시~0' + str(user3+1) + '시'
                elif user3 == 9 :
                    due = '09시~10시'
                elif user3 == 24 :
                    due = '24시~01시'
                else :
                    due = str(user3) + '시~' + str(user3+1) + '시'

                info.append(due)



    #print(info)


    Ilist, Olist, TIlist, TOlist = [], [], [], []
    message = ""

    Ires, Ores = [], []
    flag = True


    if len(info) == 0 :        #모든 호선의 정보를 얻고자 함

        Namelist = ['1호선', '2호선', '3호선', '4호선']
        message = "부산도시철도"

        for k in [1,2,3,4] :
            Ilist = data.loc[data['호선']==k][data['구분']=='승차'].iloc[:, 4:28].values.tolist()
            Olist = data.loc[data['호선']==k][data['구분']=='하차'].iloc[:, 4:28].values.tolist()


            Mean_values_tolist(Ilist, Ires)
            Mean_values_tolist(Olist, Ores)


        Namelist.append('전체')
        TIlist = data.loc[data['구분']=='승차'].iloc[:, 4:28].values.tolist()
        TOlist = data.loc[data['구분']=='하차'].iloc[:, 4:28].values.tolist()

        Mean_values_tolist(TIlist, Ires)
        Mean_values_tolist(TOlist, Ores)




    elif len(info) == 1 :        #해당 호선에 있는 모든 역의 정보를 얻고자 함

        Namelist = list(od.fromkeys(data.loc[data['호선']==info[0]]['역명'].tolist()))
        message = str(info[0]) + "호선"


        for k in range(len(Namelist)) :
            Ilist = list(data.loc[data['역명']==Namelist[k]][data['구분']=='승차'].iloc[:, 4:28].values.tolist())
            Olist = list(data.loc[data['역명']==Namelist[k]][data['구분']=='하차'].iloc[:, 4:28].values.tolist())

            Mean_values_tolist(Ilist, Ires)
            Mean_values_tolist(Olist, Ores)


        SIlist, SOlist = [], []

        Namelist.append(str(info[0]) + '호선')
        SIlist = data.loc[data['호선']==info[0]][data['구분']=='승차'].iloc[:, 4:28].values.tolist()
        SOlist = data.loc[data['호선']==info[0]][data['구분']=='하차'].iloc[:, 4:28].values.tolist()

        Mean_values_tolist(SIlist, Ires)
        Mean_values_tolist(SOlist, Ores)


        Namelist.append('전체')
        TIlist = data.loc[data['구분']=='승차'].iloc[:, 4:28].values.tolist()
        TOlist = data.loc[data['구분']=='하차'].iloc[:, 4:28].values.tolist()

        Mean_values_tolist(TIlist, Ires)
        Mean_values_tolist(TOlist, Ores)



    elif len(info) == 2 :        #해당 역의 모든 시간에 대한 정보를 얻고자 함

        Namelist = ['01시\n~02시', '02시\n~03시', '03시\n~04시', '04시\n~05시', '05시\n~06시', '06시\n~07시', '07시\n~08시', '08시\n~09시', '09시\n~10시', '10시\n~11시', '11시\n~12시', '12시\n~13시', '13시\n~14시', '14시\n~15시', '15시\n~16시', '16시\n~17시', '17시\n~18시', '18시\n~19시', '19시\n~20시', '20시\n~21시', '21시\n~22시', '22시\n~23시', '23시\n~24시', '24시\n~01시']
        message = info[1] + "역"

        Ilist = data.loc[data['역명']==info[1]][data['구분']=='승차'].iloc[:, 4:28].values.tolist()
        Olist = data.loc[data['역명']==info[1]][data['구분']=='하차'].iloc[:, 4:28].values.tolist()

        for i in range(24):
            tmp = []

            for j in range(len(Ilist)):
                tmp.append(int(str(Ilist[j][i]).replace(",", "")))

            Ires.append(round((sum(tmp)/len(tmp)) ,2))

        for i in range(24):
            tmp = []

            for j in range(len(Olist)):
                tmp.append(int(str(Olist[j][i]).replace(",", "")))

            Ores.append(round((sum(tmp)/len(tmp)) ,2))


        SIlist, SOlist = [], []

        Namelist.append(str(info[0]) + '호선')
        SIlist = data.loc[data['호선']==info[0]][data['구분']=='승차'].iloc[:, 4:28].values.tolist()
        SOlist = data.loc[data['호선']==info[0]][data['구분']=='하차'].iloc[:, 4:28].values.tolist()

        Mean_values_tolist(SIlist, Ires)
        Mean_values_tolist(SOlist, Ores)


        Namelist.append('전체')
        TIlist = data.loc[data['구분']=='승차'].iloc[:, 4:28].values.tolist()
        TOlist = data.loc[data['구분']=='하차'].iloc[:, 4:28].values.tolist()

        Mean_values_tolist(TIlist, Ires)
        Mean_values_tolist(TOlist, Ores)



    elif len(info) == 3 :        #해당 역의 특정 시간에 대한 정보를 얻고자 함

        Namelist = [info[1]]
        message = info[2]


        Ilist = data.loc[data['역명']==info[1]][data['구분']=='승차'][info[2]].tolist()
        Olist = data.loc[data['역명']==info[1]][data['구분']=='하차'][info[2]].tolist()

        Mean_tolist(Ilist, Ires)
        Mean_tolist(Olist, Ores)


        SIlist, SOlist = [], []

        Namelist.append(str(info[0]) + '호선')
        SIlist = data.loc[data['호선']==info[0]][data['구분']=='승차'][info[2]].tolist()
        SOlist = data.loc[data['호선']==info[0]][data['구분']=='하차'][info[2]].tolist()

        Mean_tolist(SIlist, Ires)
        Mean_tolist(SOlist, Ores)


        Namelist.append('전체')
        TIlist = list(data.loc[data['구분']=='승차'][info[2]].tolist())
        TOlist = list(data.loc[data['구분']=='하차'][info[2]].tolist())

        Mean_tolist(TIlist, Ires)
        Mean_tolist(TOlist, Ores)


    else :          # 예상치 못한 오류로 info에 적절치 않은 정보들이 추가로 할당되었을 경우
        print("오류!!\n프로그램을 종료합니다.")
        flag = False

    Ptype = Check("그래프 출력 유형을 번호로 입력하세요.\n그래프가 출력되길 원치 않으면 0을 입력하세요.\n\n1. 점(dot)그래프\t2. 가로막대그래프\t3. 세로막대그래프\n", 0, 4)


    print("\t-" + message + "-")

    Ndata = [x.replace("\n", "") for x in Namelist]

    DF = {
        '승차' : Ires,
        '하차' : Ores
    }

    result = pd.DataFrame(DF, index=Ndata)
    print(result)

    if Ptype == 0 :         # 그래프를 출력하지 않을 경우
        import os

        os.system("Pause")
        flag = False


    # 모든 정보가 상황에 알맞게 준비가 되고 그래프를 출력하길 원하는 경우에만 실행
    if flag :
        import matplotlib.pyplot as plt
        from matplotlib import font_manager as fm
        from matplotlib import rc

        #한글폰트 사용
        font_name = fm.FontProperties(fname="c:/Windows/Fonts/malgun.ttf").get_name()
        rc('font', family=font_name)

        Print_graph(Ptype, Ires, Ores, Namelist, message)
~~~
