# Exception 활용법

detail.html

~~~django
<form action="{% url 'polls:vote' question.id %}" method="post">
        {% csrf_token %}
        {% for choice in question.choice_set.all %}
            <input type="radio" id="choice{{ forloop.counter }}" name="choice" value="{{ choice.id }}">
            <label for="choice{{ forloop.counter }}">{{ choice.choice_text }}</label><br>
        {% endfor %}
        <input type="submit" value="Vote">
</form>
~~~





views.py

 ~~~python
# Form에서 넘어온 것 처리

def vote(request, question_id):
    selected_q = Question.objects.get(pk = question_id)
    
    try:
        selected_c = selected_q.choice_set.get(pk = request.POST['choice'])
        # selected_q.objects.get() 라고 실수를 자주 범하는데,
        # 객체의 instance에는 manager를 사용할 수 없기 때문에 유의해야 한다.
    except:
        # 에러가 발생한다면 예외처리
        return render(request, 'polls/detail.html' {
          'question': selected_q,
            'error_message': 'error',	# detail에서 폼을 다시 보여주고 에러메세지도 같이 출력
        })
    else:
        selected_c.votes += 1
        selected_c.save()
        
        # POST 데이터를 정상적으로 처리했으면 항상 리다이렉트하자!
        return HttpResponseRedirect(reverse('polls:results', args=(selected_q.id, )))
    	# HttpResponseRedirect함수는 리다이렉트할 url 주소를 인자로 받는다
 ~~~

예외처리시 context에 에러메세지를 같이 보내도 좋은 것 같다

