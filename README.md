# 기말과제

import pandas as pd
import numpy as np

def load_data(filepath): # 챗봇 데이타 로딩
    data = pd.read_csv(filepath)
    questions = data['Q'].tolist()
    answers = data['A'].tolist()

    return questions, answers

def leven_dist(my_q, data_q): #레벤슈타인 거리 함수
    n = len(my_q)
    m = len(data_q)

    table = [[0] * (m + 1) for _ in range(n + 1)]

    for i in range(1, n + 1):
        table[i][0] = i

    for j in range(1, m + 1):
        table[0][j] = j

    for i in range(1, n + 1):
        for j in range(1, m + 1):
            if my_q[i -1] == data_q[j - 1]:
                table[i][j] = table[i - 1][j -1]
            else:
                table[i][j] = 1 + min(table[i][j - 1], table[i - 1][j], table[i - 1][j -1])

    return table[n][m]

filepath = 'C:\python_work\ChatbotData.csv'

questions, answers = load_data(filepath)



while True:
    my_question = input('You: ')
    if my_question.lower() == '종료':
        break

    simil_storage = []
    for i in range(len(questions)):
        simil_coef = leven_dist(my_question, questions[i])
        simil_storage.append(simil_coef)


    best_simil = np.array(simil_storage).argmin() # 어레이로 전환 후 최소값의 주소 선택
    response = answers[best_simil]
    print('Chatbot:', response)
