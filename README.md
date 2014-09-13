Setup instructions:

- Setup node.js
- Checkout this repository - https://github.com/leenasn/jstdd_workshop.git
- Run `npm install` command


### Test Add Numbers

describe("Calculator",function(){
  it("add given 2 numbers",function(){
    calc = new Calculator();
    expect(calc.add(1,2)).toBe(3);
  })
})

function Calculator(){
}

Calculator.prototype.add = function(val1,val2){
  return val1 + val2;
}

### Fixtures

jasmine.getFixtures().fixturesPath = 'base/spec/fixtures/';

describe("Calculator App",function(){
  beforeEach(function(){
    loadFixture("calculator.html");
  });  
  it("display added number",function(){
    $("input[name='num1']").val(1);
    $("input[name='num2']").val(2);
    performCalculation();
    expect($("input[name='result']").val()).toBe(3);
  });
})

function performCalculation(){
  var num1 = $("input[name='num1']").val();
  var num2 = $("input[name='num2']").val(); 
  var output = new Calculator().add(num1,num2);
  $("input[name='result']").val(output);
}

  it("convert add given 2 numbers",function(){
    calc = new Calculator();
    expect(calc.add('1','2')).toBe(3);
  }); 
}

### Fake AJAX requests

describe("Calculate Square",function(){
  beforeEach(function(){
    server = sinon.fakeServer.create();
  });
  afterEach(function(){
    server.restore();
  });
  it("find square",function(){
    server.respondWith("GET","/square",[200,{"Content-Type":"application/json"},
    '{"result":4 }']); 

    fetchSquare();
    server.respond();
    expect(result).toBe(4);
  });
  it("return error for invalid data",function(){
    server.respondWith("GET","/square",[500,{"Content-Type":"application/json"},
    '{"error":error }']); 

    fetchSquare();

    server.respond();
    
    expect(result).toBe('error');
  });
});

function fetchSquare(){
  $.ajax({
    url: "/square",
    data: "",
    success: function(data,status,xhr){
      result = data.result;
    },
    error: function(data,status,xhr){
      result = status;
    }
  }); 
}

### Mocking

jasmine.getFixtures().fixturesPath = 'base/spec/fixtures/';

describe("Calculator App",function(){
  beforeEach(function(){
    loadFixtures("calculator.html");
    stub = sinon.stub(Calculator,"add").returns(3);
  });  
  it("display added number",function(){
    $("input[name='num1']").val(1);
    $("input[name='num2']").val(2);

    $("button").click();
    expect(stub).toHaveBeenCalled();
    $("input[name='result']").val(3);
 
  });
});


Calculator = {
  add: function(val1,val2){
    return parseFloat(val1) + parseFloat(val2);
  },

  performCalculation: function(){
    var num1 = $("input[name='num1']").val();
    var num2 = $("input[name='num2']").val(); 
    var output = this.add(num1,num2);
    $("input[name='result']").val(output);
  }
};

