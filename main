import requests
import json
import csv

Keyvalue = {}
Company = {}
linelist = {}

def GetToilet(line, station, key):

    Ans = Keyvalue.get((line, station))
    if (Ans == None):
        return 0    # 노선과 역이 존재하지 않는 경우
    Com = Company[(line, station)]
    endpoint = "https://openapi.kric.go.kr/openapi/convenientInfo/stationToilet"
    url = f"{endpoint}?serviceKey={key}&format=json&railOprIsttCd={Com}&lnCd={Ans[0]}&stinCd={Ans[1]}"

    res = requests.get(url)
    return res

if __name__ == '__main__':
    key = "$2a$10$yrwfqPFi5GGJbkuHK0Cv9u3LKVbmt..0vuziI2tx61uXM9lYiC1rO"

    f = open('C:/Users/mkms1/OneDrive/바탕 화면/학교/22.2학기/DB/data.csv', 'r', encoding ='CP949')
    rdr = csv.reader(f)
    for line in rdr:
        Keyvalue[(line[2], line[4])] = (line[3], line[5])
        Company[(line[2], line[4])] = line[1]
        if (linelist.get(line[2]) == None):
            linelist[line[2]] = ['', line[4]]   #공백 추가 안하면 딕셔너리에서 각 글자마다 value로 판단
        else:
            temp = list(linelist[line[2]])
            temp.append(line[4])
            linelist[line[2]] = temp

    f.close() 

    # 서울 1~9호선은 지역명 없이 입력
    # ㅇㅇ역일 경우 ㅇㅇ만 입력

    line = '2호선'
    station = '신도림'

    response = GetToilet(line, station, key)
    if (response != 0):
        if response.json()['header']['resultCnt'] == 0:
            print("해당 역의 화장실 정보가 존재하지 않습니다.")
        else:
            response_body = response.json()['body']
            print(f"{line} {station}역의 화장실 위치는 개찰구의 {response_body[0]['gateInotDvNm']}부에 있으며")
            print(f"{response_body[0]['dtlLoc']}에 있습니다")

        print("")
        print(f"{line}의 개찰구 내부에 화장실이 있는 역은 다음과 같습니다.")
        for data in linelist[line]:
            if data == '':
                continue
            toiletlist = GetToilet(line, data, key)
            if toiletlist.json()['header']['resultCnt'] == 0:
                continue
            elif toiletlist.json()['body'][0]['gateInotDvNm'] == '내':
                print(data)
        
    else:
        print('노선 또는 역 정보에 대한 입력이 잘못되었습니다.')
