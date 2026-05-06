# instagram
code to find out which ones in ur following list on the gram don't follow you back! don't get ur feelings hurt now


import pandas as pd
from bs4 import BeautifulSoup
import os
base_path = "/Users/seoryeonson/Downloads/instagram-son/connections/followers_and_following" #change this to ur data


def extract_usernames(html_file): 
    with open(html_file, "r", encoding="utf-8") as file:
        soup = BeautifulSoup(file, "html.parser")

    usernames = set()
    for link in soup.find_all("a", href=True):
        if "instagram.com" in link["href"]:
            username = link["href"].split("/")[-1]
            usernames.add(username)

    return usernames

followers_file = os.path.join(base_path, "followers_1.html") #follower data
following_file = os.path.join(base_path, "following.html") #following data

followers = extract_usernames(followers_file)
following = extract_usernames(following_file)

not_following_back = following - followers

df_not_following_back = pd.DataFrame(list(not_following_back), columns=["Not Following Me Back"])

df_not_following_back.to_csv("not_following_back.csv", index=False)

print("accounts that do not follow me back >:( !! :")
print(df_not_following_back)

