import requests
from bs4 import BeautifulSoup
import time
import os

res = requests.get("https://movie.naver.com/movie/running/current.naver#")
soup = BeautifulSoup(res.text,"html.parser")

def 제거(st):
    for i in "\r\n\t":
        st = st.replace(i,"")
    return st

ss영화 = []

for i in soup.select(".lst_dsc")[:10]:
    영화제목 = i.select_one(".tit > a").text
    영화평점 = i.select_one(".num").text
    관람등급 = i.select_one("span").text
    if not 관람등급:
        관람등급 = "없음"
    B = i.select(".info_txt1 > dd")
    참여자수 = i.select_one(".num2 > em").text
    개요 = 제거(B[0].text)
    영화감독 = 제거(B[1].text)
    A = 개요.split("|")
    영화장르 = A[0]
    상영시간 = A[1]
    개봉날짜 = A[2]
    영화.append([영화제목,영화평점,관람등급,참여자수,영화감독,영화장르,상영시간,개봉날짜])
while True:
    print("="*30)
    print("최신영화 top10")
    print("="*30)
    for num, i in enumerate(영화,1):
        print(f"{num}, {i[0]}")
    menu = int(input("영화선택 (종료0)> "))

    if menu == 0:
        print("너 서운해:(")
        break
    
    time.sleep(0.7)
    os.system("cls")

    선택영화 = 영화[menu-1]
    print("="*30)
    print(f"{선택영화[0]} INFO")
    print("="*30)
    print(f"영화평점\t{선택영화[1]}")
    print(f"영화등급\t{선택영화[2]}")
    print(f"참여자수\t{선택영화[3]}")
    print(f"영화감독\t{선택영화[4]}")
    print(f"영화장르\t{선택영화[5]}")
    print(f"영화시간\t{선택영화[6]}")
    print(f"개봉날짜\t{선택영화[7]}")
    input("\n\n메인으로(EENTER)")
