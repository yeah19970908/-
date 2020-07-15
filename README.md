# 数据挖掘小组作业
MF1915080 崔洁  MF1915115 闫智  MF1915124 郑茗文
一、基于核密度估计的时间序列异常检测
1.1 原理
   # 创建𝐿∗𝑀的矩阵，记为𝑍
    def create_matrix(self, t):
        matrix = []
        for i in range(self.M):
            vector = self.data_value[t-self.M-self.L+1+i:t-self.M+1+i]
            matrix.append(vector)
        return matrix
    
   # 计算NCM值
    def NCM(self, Z_Cl, Z_T):
        dis = []
        for i in range(self.T):
            dist = np.linalg.norm(np.array(Z_Cl) - np.array(Z_T[i]))
            dis.append(dist)
        return sum(sorted(dis)[:self.k])
   # 后C列的NCM值构成一个列表al
    def create_al(self, Z_C, Z_T):
        al = []
        for i in range(self.C):
            al.append(self.NCM(Z_C[i], Z_T))
        return al
   # 计算异常分数
    def cal_score(self, a, al):
        s = 0
        for i in range(len(al)):
            if a > al[i]:
                s += 1
        return s / len(al)
           def predict(self):
        score = [0]*(self.L+self.M-1)   # 短于时间窗口的数据默认为正常值
        for t in range(self.L+self.M-1, len(self.data_value)):
            matrix = self.create_matrix(t)
            Z_T = matrix[:self.T]
            Z_C = matrix[self.T:]
            al = self.create_al(Z_C, Z_T)
            z = self.data_value[t-self.L+1:t+1]
            a = self.NCM(z, Z_T)
            score_a = self.cal_score(a, al)
            score.append(score_a)
        self.data['anomaly_score'] = score
        delta = np.array(self.data['label']) - np.array(self.data['anomaly_score'])
        MSE = np.linalg.norm(delta) / len(delta)
        return self.data,MSE
