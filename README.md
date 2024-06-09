    # 기말과제
    
    import pandas as pd
    import numpy as np
    
    def load_data(filepath): # 챗봇 데이타 로딩 함수
        data = pd.read_csv(filepath)
        questions = data['Q'].tolist()
        answers = data['A'].tolist()
    
        return questions, answers
    
    def leven_dist(my_q, data_q): # 레벤슈타인 거리 함수
        n = len(my_q) # 질문자 질문의 길이
        m = len(data_q) # 입력된 데이터의 길이
    
        table = [[0] * (m + 1) for _ in range(n + 1)] #매트릭스 테이블 생성 및 0으로 초기화
    
        for i in range(1, n + 1): #첫 열의 각 행 값을 순차적으로 설정
            table[i][0] = i
    
        for j in range(1, m + 1): #첫 행의 각 열 값을 순차적으로 설정
            table[0][j] = j
    
        for i in range(1, n + 1):
            for j in range(1, m + 1):
                if my_q[i -1] == data_q[j - 1]:
                    table[i][j] = table[i - 1][j -1]
                else:
                    table[i][j] = 1 + min(table[i][j - 1], table[i - 1][j], table[i - 1][j -1])
                    # 대각 방향, 위 방향, 완쪽 방향의 값들 중 최소값에 1을 증가시킴
        return table[n][m]
    
    filepath = 'C:\python_work\ChatbotData.csv' # 데이터 화일이 있는 곳의 경로
    
    questions, answers = load_data(filepath) # 데이터 화일을 questions와 answers로 분리하여 적재
    
    
    
    while True:
        my_question = input('You: ')
        if my_question.lower() == '종료': # '종료' 입력으로 무한루프 종료
            break
    
    
        simil_storage = [] #레벤슈타인 거리 함수 실행 후 생성된 값의 저장소
        for i in range(len(questions)):
            simil_coef = leven_dist(my_question, questions[i]) # 질문자의 questions 과 데이터의 questions의 i번째를 비교하여 
                                                               # 레벤슈타인 거리 함수 실행
            simil_storage.append(simil_coef) #생성된 값을 저장소에 입력
    
    
        best_simil = np.array(simil_storage).argmin() # 저장소의 값을 어레이로 전환 후 최소값의 주소 선택
        response = answers[best_simil]
        print('Chatbot:', response)
    
