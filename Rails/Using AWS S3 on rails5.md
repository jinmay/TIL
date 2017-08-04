# rails5 에서 AWS S3 사용하기

**멋쟁이 사자처럼 5기에서 배운 것을 토대로 작성**

1. AWS S3 에서 새로은 bucket을 만든다
2. rails에서 **carrierwave** / **fog-aws** **gem** 을 설치한다

~~~ruby
# vi Gemfile 을 통해 gem 을 추가하고 bundle install 하자
gem 'carrierwave' # rails에서 file handling을 도와줌
gem 'fog-aws' # S3을 사용하기 위해

bundle install 
~~~

3. initializers에 적절한 이름으로 rb 파일을 만들고 carrierwave 설정을 해준다

> 참고로 initializers의 파일들은 레일즈 서버가 돌아가는 최초에 딱 한번 실행된다

설정내용은 [carrierwave github - using aws s3]((https://github.com/carrierwaveuploader/carrierwave)) 을 참고하자

~~~ruby
CarrierWave.configure do |config|
  config.fog_provider = 'fog/aws'                        # required
  config.fog_credentials = {
    provider:              'AWS',                        # required
    aws_access_key_id:     'xxx',                        # required
    aws_secret_access_key: 'yyy',                        # required
    region:                'eu-west-1',                  # optional, defaults to 'us-east-1'
    host:                  's3.example.com',             # optional, defaults to nil
    endpoint:              'https://s3.example.com:8080' # optional, defaults to nil
  }
  config.fog_directory  = 'name_of_directory'                          # required
  config.fog_public     = false                                        # optional, defaults to true
  config.fog_attributes = { cache_control: "public, max-age=#{365.day.to_i}" } # optional, defaults to {}
end
~~~

수정해야 할 부분은, 

key_id / key / region / endpoint / for_directory 정도가 되겠다

AWS region과 endpoint에 대한 정보는 [http://docs.aws.amazon.com/general/latest/gr/rande.html](http://docs.aws.amazon.com/general/latest/gr/rande.html) 를 참고하도록 하자



4. 업로더를 도와줄 uploader를 생성한다

~~~bash
rails g uploader <uploader_name> # app/uploader 경로에 해당 이름의 Uploader가 생성된다
~~~

5. uploader.rb 

uplaoder.rb 의 내용을 바꿔주어야 한다

**fog**를 사용할 것이므로 **storage :fog**의 주석을 풀어주고

저장 경로를 설정하기 위해 **def store_dir**의 내용을 적절히 바꾸어 준다

~~~ruby
# uploader.rb

storage :fog

def store_dir
  "uploads/"
end
~~~

6. 파일을 업로드 할 form 을 만든다

application.rb의 보안설정도 일단 꺼둔다

~~~html
<h1>New upload</h1>
<hr>
<form action="/home/upload"  method="post" enctype="multipart/form-data">
    <input type="file" name="picture" accept="/image/*">
    <input type="submit" value="upload">
</form>
~~~

7. routes.rb 

~~~ruby
# routes.rb
get 'home/index'
post 'home/upload'
~~~

8. controller에서 처리

~~~ruby
# home_controller.rb
def index
end

def upload
  # index.html 에서 업로드한 파일을 params을 통해 가져와 file 변수에 저장하고
  # uploader를 만든 후
  # S3에 업로드 하고 적절한 곳으로 redirect 한다
  file = params[:picture]
  
  uploader = TestUploader.new
  uploader.store!(file)
  
  redirect_to 'index'
end
~~~

> 파일명을 바꾸고 싶다면 uploader.rb의 **def filename**을 수정하면 된다