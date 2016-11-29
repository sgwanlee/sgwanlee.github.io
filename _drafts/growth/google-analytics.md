
Google sheet - Google anlaytics add-on

- Metrics에 빈줄이 있으면 안됩니다.
- filter의 operator는 string형태로 바로 사용하면 됩니다. (==, >, <)
- operator와 dimension사이에 빈칸있으면 안됩니다.
- 내림차순 정렬은 앞에 '-'를 붙입니다.
- 

filter를 AND조건으로 붙일 때는 filter 사이에 ';'(세미콜론)을 붙입니다.

    ga:channelGrouping==Referral;ga:sessions>10

Query Test
https://ga-dev-tools.appspot.com/query-explorer/

- 두개의 column이 일치하는 row의 특정 column데이터를 가져오기
=index(filter('방문자획득-그래프-organic'!$A:$C, '방문자획득-그래프-organic'!$A:$A = $A26),2)

https://productforums.google.com/forum/#!topic/docs/6B-wbF1Om1g


특정 cell에서 마지막 cell까지 값을 가져오

    filter('방문자획득-그래프-organic'!$A$16:$A, isblank('방문자획득-그래프-organic'!$A$16:$A) = FALSE) 


"<>" : not equal



dimension/metric explorer
https://developers.google.com/analytics/devguides/reporting/core/dimsmets