# GNN-Based Real-Time Fraud Detection System (IEEE-CIS Dataset)

## Project Overview

This project implements a Graph Neural Network (GNN) based system for fraud detection using the IEEE-CIS Fraud Detection dataset. The core idea is to represent transactions and their relationships as a graph, allowing the GNN to learn complex patterns indicative of fraudulent activity that traditional methods might miss. By modeling connections based on shared entities (cards, emails, devices), temporal proximity, and feature similarity, the system aims to identify sophisticated fraud schemes.

The project covers:
1.  **Data Preprocessing:** Handling missing values, feature engineering, encoding categorical features, and normalizing numerical data from the IEEE-CIS dataset.
2.  **Graph Construction:** Transforming tabular transaction data into a heterogeneous graph where transactions are nodes and edges represent various relationships.
3.  **GNN Architecture:** Designing a `HeteroGNN` using PyTorch Geometric, capable of processing different types of relationships (edge types).
4.  **Model Training & Evaluation:** Training the GNN on labeled data, handling class imbalance, and evaluating performance using fraud-detection-specific metrics (AUCPR, F1-score, Precision, Recall).
5.  **Real-Time Considerations (Conceptual):** Discussing the architectural aspects required for deploying such a system in a real-time environment.

## Tech Stack & Tools

*   **Programming Language:** Python 3.x
*   **Core Libraries:**
    *   **PyTorch:** For building and training neural networks.
    *   **PyTorch Geometric (PyG):** For creating and working with graph neural networks.
    *   **Pandas:** For data manipulation and analysis.
    *   **NumPy:** For numerical operations.
    *   **Scikit-learn:** For preprocessing (scaling, encoding) and evaluation metrics.
    *   ** FAISS: ** For ANN similarity search for edge creation
*   **Data Visualization:** Matplotlib, Seaborn (for confusion matrix).
*   **Dataset:** [IEEE-CIS Fraud Detection Dataset](https://www.kaggle.com/c/ieee-fraud-detection) (requires download from Kaggle).

## Key Concepts & System Flow

### 1. High-Level System Flow

The system processes transactions by first transforming them into a graph structure and then using a GNN to predict fraud.

[![](https://mermaid.ink/img/pako:eNqNVW1v2jAQ_iuWp01Mcl-SkACpNAmSvlPala4fBtXkJUdqNTiR43RlVf_7HJuEMCZ1QQJ899xzd8_ZziuOshiwjxNB80d0F845Us9wdkt_oTtBeUEjyTKOQiopCqb3xQPa2_uCRp1xRmP0CV2BSEB7Px-Z2JEGBJ0bAbnIIigKxhOFPAEqSwHomCeMAwhlrUMCHRJ2TnURQcYLKUqdt0IYTFH-XNcoKOMq-IblkComNKstqDY9mJDqiZkA08H4dmMNf8gqZmYSVtWbts460zxl0uQ4uKfpwR0Usi6zes4UrinBqFIFHhvCzulkgq6UommDaQevURWFAQ2FZAulcKFJdAjE2jWVmYBOZ21CDe_nmg94vCPNOV-AAB7BRpvG9P_iXMwmsD38qRRAl0aiy9ZgkQa-tFu81KBxJ1xxumQRMgp_y2MqAR2gaV1qoBjrAdex40qYryWI1b_laEF3fBtNzThY1XdrHI0OOwlNhYtMbDBtjq3xaYv2ns5OBC1jNI2q5AcohIgVqp-H3fGEm25a23aNu9DOncE12_7jR6X-Kq02W1VilFLB5Mr41KIoQliguNqGC5am_ofQ7brWiKgDlD2B_8G1reAwXC_3frFYPvp2_nL0F0E9T8PhOE636zUcvZN-MBy-y7HUOhuGnjPoeq0qnF7ovl9FoQdpGPpDO3Q2DF7XGtnH_2JocaAhudBatInRiAQkJGfkkozrPrf860NJ6unqPrYQu5utaG1HgzmtE5vPxnHO81Ku776CXJdSLcdZwtSZZ9lEUR5to5lkNB2rfFSQoTp-z_qYWOSE8cY-gRc5pisQ18IQ1o1p4xbhGUgQmbpSny0Uqe9RmkVPW4irIrlRvz-O4wTuVjlY5G-LvWOZkGGSCEjUobZI1YTu0kIxSMpSkwMT9VJhMfbVXQ4EL0EsabXEr1X2OZaPsIQ59tXfmIqnOZ7zNxWTU_49y5Z1mMjK5BH7C5oWalXqeyRkVF0iy8aqTk4MIshKLrHvOn1Ngv1X_IJ9Z2DtO06_13M9p9vzXI_gFfYH9r7b9dQ-dyx70O_b_TeCf-ush_sD-9Dtez3X9QaHA_WXYIiZmveVeVHq9-XbH-FbT1k?type=png)](https://mermaid.live/edit#pako:eNqNVW1v2jAQ_iuWp01Mcl-SkACpNAmSvlPala4fBtXkJUdqNTiR43RlVf_7HJuEMCZ1QQJ899xzd8_ZziuOshiwjxNB80d0F845Us9wdkt_oTtBeUEjyTKOQiopCqb3xQPa2_uCRp1xRmP0CV2BSEB7Px-Z2JEGBJ0bAbnIIigKxhOFPAEqSwHomCeMAwhlrUMCHRJ2TnURQcYLKUqdt0IYTFH-XNcoKOMq-IblkComNKstqDY9mJDqiZkA08H4dmMNf8gqZmYSVtWbts460zxl0uQ4uKfpwR0Usi6zes4UrinBqFIFHhvCzulkgq6UommDaQevURWFAQ2FZAulcKFJdAjE2jWVmYBOZ21CDe_nmg94vCPNOV-AAB7BRpvG9P_iXMwmsD38qRRAl0aiy9ZgkQa-tFu81KBxJ1xxumQRMgp_y2MqAR2gaV1qoBjrAdex40qYryWI1b_laEF3fBtNzThY1XdrHI0OOwlNhYtMbDBtjq3xaYv2ns5OBC1jNI2q5AcohIgVqp-H3fGEm25a23aNu9DOncE12_7jR6X-Kq02W1VilFLB5Mr41KIoQliguNqGC5am_ofQ7brWiKgDlD2B_8G1reAwXC_3frFYPvp2_nL0F0E9T8PhOE636zUcvZN-MBy-y7HUOhuGnjPoeq0qnF7ovl9FoQdpGPpDO3Q2DF7XGtnH_2JocaAhudBatInRiAQkJGfkkozrPrf860NJ6unqPrYQu5utaG1HgzmtE5vPxnHO81Ku776CXJdSLcdZwtSZZ9lEUR5to5lkNB2rfFSQoTp-z_qYWOSE8cY-gRc5pisQ18IQ1o1p4xbhGUgQmbpSny0Uqe9RmkVPW4irIrlRvz-O4wTuVjlY5G-LvWOZkGGSCEjUobZI1YTu0kIxSMpSkwMT9VJhMfbVXQ4EL0EsabXEr1X2OZaPsIQ59tXfmIqnOZ7zNxWTU_49y5Z1mMjK5BH7C5oWalXqeyRkVF0iy8aqTk4MIshKLrHvOn1Ngv1X_IJ9Z2DtO06_13M9p9vzXI_gFfYH9r7b9dQ-dyx70O_b_TeCf-ush_sD-9Dtez3X9QaHA_WXYIiZmveVeVHq9-XbH-FbT1k)

### 2. Graph Construction Strategy

Transactions are represented as nodes. Edges define relationships between them:

    Node Type: transaction

    Edge Types (Examples):

        shared_card_identity: Transactions sharing the same (hashed/combined) card details.

        shared_email_P: Transactions with the same payer email domain.

        shared_device_info: Transactions originating from the same device fingerprint.

        temporal_proximity: Transactions occurring within a short time window of each other.

        similar_amount: Transactions with numerically similar transaction amounts (identified via k-NN).

[![](https://mermaid.ink/img/pako:eNpdklFP6yAUx78KOcZEE7a0QNeNJb64-KQ3N9qne2saUthKbKGhTDeXfXfZaucmTxz-v_M_Bzg7KK1UwGHlRFuhx-fcoLCy-P9N5oTpROm1NSi-ff0WyKVATgK9FOhJYJcCOwnJpZAchKE8Go1QVwmnZFEKJwstlfHab8P5XWhifs551bTWibpond3o5kTRgSJnblK961IV2ixtT7GBokcq5NfCFaKxa-N7IhkIduajGqHr4m9PxPOh8-tr9OK3tTYrZMLDdv1pWYuuW6gl8j_3_RNktNR1za_uF-kDJbjzzr4pfkUp_d6PPrT0FSftZn5mFOrhjOCM4ozhLPltOgccflNL4N6tFYZGudBrCGF3MMnBV6pROfCwlcK95ZCbfchphflnbTOkObteVcCXou5CtG6l8GqhRZiTH0QZqdz94aWAT48OwHewAT6ZjQkhNKWzCWERoQzDFjiLxjQmbDJjU8JIPE2TPYbPY81oHKIoLEqnJKFplGJQUnvrnvr5PI7p_guKUtJh?type=png)](https://mermaid.live/edit#pako:eNpdklFP6yAUx78KOcZEE7a0QNeNJb64-KQ3N9qne2saUthKbKGhTDeXfXfZaucmTxz-v_M_Bzg7KK1UwGHlRFuhx-fcoLCy-P9N5oTpROm1NSi-ff0WyKVATgK9FOhJYJcCOwnJpZAchKE8Go1QVwmnZFEKJwstlfHab8P5XWhifs551bTWibpond3o5kTRgSJnblK961IV2ixtT7GBokcq5NfCFaKxa-N7IhkIduajGqHr4m9PxPOh8-tr9OK3tTYrZMLDdv1pWYuuW6gl8j_3_RNktNR1za_uF-kDJbjzzr4pfkUp_d6PPrT0FSftZn5mFOrhjOCM4ozhLPltOgccflNL4N6tFYZGudBrCGF3MMnBV6pROfCwlcK95ZCbfchphflnbTOkObteVcCXou5CtG6l8GqhRZiTH0QZqdz94aWAT48OwHewAT6ZjQkhNKWzCWERoQzDFjiLxjQmbDJjU8JIPE2TPYbPY81oHKIoLEqnJKFplGJQUnvrnvr5PI7p_guKUtJh)

### 3. GNN Architecture (HeteroGNN)

A Heterogeneous GNN (HeteroConv in PyG) is used to process the graph with multiple edge types. Each edge type can have its own message passing mechanism (e.g., using SAGEConv or GATConv). The information from different relationship types is then aggregated.

[![](https://mermaid.ink/img/pako:eNqVVm2P2jgQ_isjV5VaXUAkEF6CVKldur1KW1pd0X24ZbXyJiZYJHZkO3twiP_eySsO3KraiA8z5plnhvEzE44klBEjAYkVzbawWqwF4PNVZLm5ZdTkiun7JUKg8aAHK0WFpqHhUugH6PU-IJ4bTpM7LhhV72oPKhfu6IEp-AM-YsQzLaLez9eiSqTzpyrzl-WyxGm4R7OK0Q8VqHgirliZEVafzqclzP0q7r_nBiuGjZJpt5iqvj-ZYUreSPHsHo9npy7NPZ3agjpFWWGPC2YoT7C82gjgkse1yu2WfPdX95uioWWL3aq3n9MnFkVcxHU7v-n4B9X68XMUs9UhY-49nkBxhBi8gPY8ANaP-w7oLVUsegypih7mL-X6X2rvBWqvoTYszaSiySt5ly_wLgPo9_uXZFc_uOT8GMeKxdQw99iY2M3Tb2K9i9jfwJcvw5mIzs7ZevsW_uY6p0lygISLnS0TeEpkuAMjgRsNUSWVTuhqyzXgJ5WKQShFyDKDXEB1LZhCfr1WgkWCooMhFfDEwCge7g4dQjv7hdYb0c6hzsv2WcJDbqq6Ux5vDTLnmmFFOmfaAbkxTABPEccieygu05yn2X13tnHMF0pmMjfvrUZa2DJ2yfamHJnvqhrcI2oCbnNltjiO5_kHqaCebAQ0F99eS7OrrHEv6ZutMK8RV-lK1C0X7cYq7c6-asq3UGVURXAnY7zfZutUHor8VtE8gp8hXu1Du1Hwin6aAzY8rvwwQfkt2Aa4LId_w5MkeDOYDJk7drRRcseCN8PhsLZ7__LIbAMv288v4jMlQ6Z1tcMqFgy7cV9Fggp8_lRqtmJICk3gLDDxKppK6TbRYORNWwrf97sUbrZvDiJabC9FDwH44Jdts8i7ryLHbn_dwHkXbYnBsXTnWBfpXAvC7mWH0JZ926sO4mp3OVcb6epk6ZwXjmNtUquNc-LgS5lHJDAqZw5JmUpp4ZJjkX1NcFZStiYBmhFVuzVZixPGZFT8I2XahCmZx1sSbGii0cuzCFMuOMXlkranCieKqRuZC0MCz_dKEhIcyZ4Es0l_NB4Nx8PBbOqORv7QIQcSDCd9f-aNp_54NhuNvens5JD_yqyD_nQymuEz9GYTbzBwJw5hETdSfav-aJT_N06_AKcNwds?type=png)](https://mermaid.live/edit#pako:eNqVVm2P2jgQ_isjV5VaXUAkEF6CVKldur1KW1pd0X24ZbXyJiZYJHZkO3twiP_eySsO3KraiA8z5plnhvEzE44klBEjAYkVzbawWqwF4PNVZLm5ZdTkiun7JUKg8aAHK0WFpqHhUugH6PU-IJ4bTpM7LhhV72oPKhfu6IEp-AM-YsQzLaLez9eiSqTzpyrzl-WyxGm4R7OK0Q8VqHgirliZEVafzqclzP0q7r_nBiuGjZJpt5iqvj-ZYUreSPHsHo9npy7NPZ3agjpFWWGPC2YoT7C82gjgkse1yu2WfPdX95uioWWL3aq3n9MnFkVcxHU7v-n4B9X68XMUs9UhY-49nkBxhBi8gPY8ANaP-w7oLVUsegypih7mL-X6X2rvBWqvoTYszaSiySt5ly_wLgPo9_uXZFc_uOT8GMeKxdQw99iY2M3Tb2K9i9jfwJcvw5mIzs7ZevsW_uY6p0lygISLnS0TeEpkuAMjgRsNUSWVTuhqyzXgJ5WKQShFyDKDXEB1LZhCfr1WgkWCooMhFfDEwCge7g4dQjv7hdYb0c6hzsv2WcJDbqq6Ux5vDTLnmmFFOmfaAbkxTABPEccieygu05yn2X13tnHMF0pmMjfvrUZa2DJ2yfamHJnvqhrcI2oCbnNltjiO5_kHqaCebAQ0F99eS7OrrHEv6ZutMK8RV-lK1C0X7cYq7c6-asq3UGVURXAnY7zfZutUHor8VtE8gp8hXu1Du1Hwin6aAzY8rvwwQfkt2Aa4LId_w5MkeDOYDJk7drRRcseCN8PhsLZ7__LIbAMv288v4jMlQ6Z1tcMqFgy7cV9Fggp8_lRqtmJICk3gLDDxKppK6TbRYORNWwrf97sUbrZvDiJabC9FDwH44Jdts8i7ryLHbn_dwHkXbYnBsXTnWBfpXAvC7mWH0JZ926sO4mp3OVcb6epk6ZwXjmNtUquNc-LgS5lHJDAqZw5JmUpp4ZJjkX1NcFZStiYBmhFVuzVZixPGZFT8I2XahCmZx1sSbGii0cuzCFMuOMXlkranCieKqRuZC0MCz_dKEhIcyZ4Es0l_NB4Nx8PBbOqORv7QIQcSDCd9f-aNp_54NhuNvens5JD_yqyD_nQymuEz9GYTbzBwJw5hETdSfav-aJT_N06_AKcNwds)
