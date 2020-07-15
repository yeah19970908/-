# 数据挖掘小组作业
#MF1915080 崔洁  MF1915115 闫智  MF1915124 郑茗文
#一、基于核密度估计的时间序列异常检测
#1.1 原理
   # 创建𝐿∗𝑀的矩阵，记为𝑍
    def create_matrix(self, t):
        matrix = []
        for i in range(self.M):
            vector = self.data_value[t-self.M-self.L+1+i:t-self.M+1+i]
            matrix.append(vector)
        return matrix
