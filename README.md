# basic-crackme-reversal
This is a reversal write up of a extremely easy and basic crackme on crackmes.one with a keygen made in C++

# Reversal
(All function names and variables have been renamed)

In the main function of this program we see a call to a `start` function 
![image](https://user-images.githubusercontent.com/97218884/182189856-702030be-4135-45f8-94bc-66e3a583f1dd.png)

Upon entering the start function we see this function is asking for user input, in the form of a 10 character string and then saves that input to a `username` string 
![image](https://user-images.githubusercontent.com/97218884/182190083-140065d8-dcea-4fd7-b7a7-48eafb04edcf.png)

We then see a call to a `encrypt_username` function.

First look at this function and it's quite simple, it takes each character of the given `username` and adds the current index value and adds 101 to it.

![image](https://user-images.githubusercontent.com/97218884/182190515-35a8ac91-2562-445c-ac17-155aec1c3131.png)

Inside the `encrypt_username` function we see a call to `check_key` and inside this check_key function it takes in user input and uses `strcmp` on the given user input with the encrypted username and checks if the result is 1.

Making a keygen for this is very easy, it just requires you to replicate the `encrypt_username` function's encryption and then increment the first character of the encrypted string (could be any character). This will increment its ASCII code 

```cpp

#include <iostream>
#include <string_view>

std::pair<std::string, std::string> keygen( std::string_view name )
{
    std::string encrypted_name{ 10, '\0' };

    for ( auto i = 0; ; ++i )
    {
        const auto v0 = name.size( ) > i;
        if ( (v0 & ( name.size( ) > i ) ) == 0)
            break;
        encrypted_name[ i ] = name[ i ] + i + 101;
    }

    std::string key{ encrypted_name };
    key[ 0 ] =  encrypted_name[ 0 ] + 1;
    return { encrypted_name, key };
    
}

int main()
{

    const auto [ encrypted_name, key ] = keygen( "key" );

    std::printf( "%s\n%s", encrypted_name.data( ), key.data( ) );

	return 0;
}
```

