# Timezone setting

레일즈에서 타임존을 관리하는 방법은 아래의 두 가지라고 한다

~~~ruby
config.time_zone = 'Seoul'  # rails 상에서 사용될 타임존
config.active_record.default_timezone = :local # active record 타임존
~~~

#### config.time_zone

Rails 상에서 사용 될 타임존이고, 기본값은 **utc**이다



#### config.active_record.default_timezone

이름에서 나타내듯이 ActiveRecord에서 사용할 타임존이고, 마찬가지로 기본값은 **utc**이다

데이터베이스에는 DATETIME 자료형에 시간대 정보가 없다고한다. 즉, DB에 저장되어있는 날짜(2017-08-02)의 값이 UTC인지  local인지 알 수 없다는 것이다. 때문에 알아서 설정을 해주어야 하는데 ActiveRecord의 경우 위와 같은 방식으로 설정할 수 있다

가급적이면 기본설정인 **:utc**을 그대로 따라가는 편이 낫다. DB에는 **:utc**로 저장해놓고, 사용할 때 자신의 타임존 **:local**으로 변경하여 사용하는 것이 유리하다고 한다

