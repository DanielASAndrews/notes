# The Little Mocker

## Author: Uncle Bob

### Tags: 

@coding

[ref](https://blog.cleancoder.com/uncle-bob/2014/05/14/TheLittleMocker.html)

### Questions

- What is a mock, dummy, spy, stub, fake?

### Content

This is a dummy:

```
public class DummyAuthorizer implements Authorizer {
  public Boolean authorize(String username, String password) {
	return null;
  	  }
}
```

You pass it into something when you don’t care how it’s used.  As part of a test, when you must pass an argument, but you know the argument will never be used.

This is a stub:

```
public class AcceptingAuthorizerStub implements Authorizer {
  public Boolean authorize(String username, String password) {
	return true;
  	  }
}
```

Well, suppose you want to test a part of your system that requires you to be logged in.  But you already know that login works, You’ve tested it a different way. Why test it again?  Because it’s easy? ... But it takes time. And it requires setup. And if there’s a bug in login, your test will break. And, after all, it’s an unnecessary coupling.

This is a spy:

```
public class AcceptingAuthorizerSpy implements Authorizer {
  public boolean authorizeWasCalled = false;

  public Boolean authorize(String username, String password) {
	authorizeWasCalled = true;
	return true;
  }
}
```

You’d use this when you wanted to be sure that the authorize method was called by your system.  **You have to be careful.** The more you spy, the tighter you couple your tests to the implementation of your system. And that leads to fragile tests.  What’s a fragile test?  A test that breaks for reasons that shouldn’t break a test.  Well if you change the code in the system, some tests are going to break.  Yes, but well designed tests minimize that breakage. Spies can work against that.

This is a mock:

```
public class AcceptingAuthorizerVerificationMock implements Authorizer {
  public boolean authorizeWasCalled = false;

  public Boolean authorize(String username, String password) {
	authorizeWasCalled = true;
	return true;
  }

  public boolean verify() {
	return authorizedWasCalled;
  }
}
```

The mock is not so interested in the return values of functions. It’s more interested in what function were called, with what arguments, when, and how often.  A mock spies on the behavior of the module being tested. And the mock knows what behavior to expect.

This is a fake:

```
public class AcceptingAuthorizerFake implements Authorizer {
	  public Boolean authorize(String username, String password) {
		return username.equals("Bob");
	  }
  }
```

A Fake has business behavior. You can drive a fake to behave in different ways by giving it different data.  
> Yes, Hmmm. I don’t often write fakes. Indeed, I haven’t written one for over thirty years. Wow! So what do you write? Do you use all these other test doubles? Mostly I use stubs and spies. And I write my own, I don’t often use mocking tools. Do you use Dummies? Yes, but rarely. What about mocks? Only when I use a mocking tool. But you said you don’t use mocking tools. That’s right, I usually don’t. Why not? Because stubs and spies are very easy to write. My IDE makes it trivial. I just point at the interface and tell the IDE to implement it. Voila! It gives me a dummy. Then I just make a simple modification and turn it into a stub or a spy. So I seldom need the mocking tool. So it’s just a matter of convenience? Yes, and the fact that I don’t like the strange syntax of mocking tools, and the complications they add to my setups. I find writing my own test doubles to be simpler in most cases.

