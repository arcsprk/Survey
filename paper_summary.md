
![[Pasted image 20241016120512.png]]


# Summary

This paper introduces a novel approach called ReSP (Retrieve, Summarize, Plan) for multi-hop question answering using iterative Retrieval-Augmented Generation (RAG). Here are the key points:

## Main Contributions

1. ReSP incorporates a dual-function summarizer to address context overload and redundant planning issues in iterative RAG methods.
2. The summarizer creates two types of memory:
    
    - Global evidence memory: Summarizes information relevant to the main question
    - Local pathway memory: Generates responses for sub-questions
    
3. ReSP significantly outperforms existing methods on multi-hop QA benchmarks:
    
    - 4.1 F1 score improvement on HotpotQA
    - 5.9 F1 score improvement on 2WikiMultihopQA
    

## Methodology

- ReSP consists of four components: Reasoner, Retriever, Summarizer, and Generator.
- The iterative process involves:
    
    1. Retrieving relevant documents
    2. Summarizing and updating memory queues
    3. Determining if information is sufficient to answer the main question
    4. Generating a new sub-question if needed, or producing the final answer
    

## Experimental Results

- ReSP achieves state-of-the-art performance on HotpotQA and 2WikiMultihopQA datasets.
- The approach demonstrates robustness to context length variations.

## Analysis

- Main Results
	- token-level F1 score

| Method       | Pipeline  | HotpotQA | 2Wiki |
| ------------ | --------- | -------- | ----- |
| Standard RAG | Single    | 38.6     | 20.1  |
| SuRe         | Single    | 33.4     | 20.6  |
| RECOMP       | Single    | 37.5     | 32.4  |
| REPLUG       | Single    | 31.2     | 21.1  |
| Iter-RetGen  | Iterative | 38.3     | 21.6  |
| IRCoT        | Iterative | 43.1     | 32.4  |
| ReSP(ours)   | Iterative | 47.2     | 38.3  |

|   |   |   |   |   |
|---|---|---|---|---|
|Reasoner|Summarizer|Generator|HotpotQA|2Wiki|
|Llama3-8B-Instruct|Llama3-8B-Instruct|Llama3-8B-Instruct|47.2|38.3|
|Llama3-70B-Instruct|Llama3-8B-Instruct|Llama3-8B-Instruct|48.8(+1.6pt)|37.2(-1.1pt)|
|Llama3-8B-Instruct|Llama3-70B-Instruct|Llama3-8B-Instruct|47.3(+0.1pt)|34.1(-4.2pt)|
|Llama3-8B-Instruct|Llama3-8B-Instruct|Llama3-70B-Instruct|48.2(+1.0pt)|38.7(+0.4pt)|

- Larger base models improve performance for the Generator module but not necessarily for the Reasoner and Summarizer.
- ReSP effectively mitigates over-planning and repetitive planning issues in multi-hop question answering
    

![[Pasted image 20241016120543.png]]

# IRCoT과 비교

## 성능 향상

1. ReSP는 HotpotQA 데이터셋에서 47.2 F1 점수를 달성하여 IRCoT의 43.1보다 4.1점 높은 성능을 보임
2. 2WikiMultihopQA 데이터셋에서도 ReSP는 38.3 F1 점수를 기록하여 IRCoT의 32.4보다 5.9점 높은 성능을 달성함

## 과도한 계획 방지

ReSP는 IRCoT에 비해 과도한 계획(over-planning)을 효과적으로 방지함

1. IRCoT는 정보 처리와 계획을 한 단계에서 통합하기 때문에 작업 복잡성이 높아 중요한 정보를 놓치는 경우가 있음
2. 반면 ReSP는 요약기(summarizer)를 통해 주요 질문과 관련된 지원 사실들을 정확하고 포괄적으로 획득함

## 반복적 계획 방지

ReSP는 IRCoT와 달리 반복적 계획(repetitive planning)을 효과적으로 방지

1. IRCoT는 이전 검색 기록을 유지하지 않아 동일한 하위 질문을 반복적으로 생성할 수 있음
2. ReSP는 이전에 검색한 하위 질문을 출력하지 않도록 설계되어 있어, 새로운 정보를 찾지 못할 경우 검색 주제를 조정함

## 컨텍스트 길이에 대한 강건성

ReSP는 컨텍스트 길이 변화에 대해 더 강건한 성능을 보임

1. 검색된 문서 수를 조정하는 실험에서 ReSP는 표준 RAG와 IRCoT에 비해 더 안정적인 성능을 유지
2. 이는 ReSP가 컨텍스트 과부하 문제를 효과적으로 해결할 수 있음을 시사함
    
결론적으로, ReSP는 요약과 계획 단계를 분리하고 메모리 큐를 활용함으로써 IRCoT의 한계를 극복하고 다중 홉 질문 답변 작업에서 더 우수한 성능을 달성함
