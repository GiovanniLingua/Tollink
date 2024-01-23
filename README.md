unit CalculatorForm;

interface

uses
  Windows, Messages, SysUtils, Variants, Classes, Graphics, Controls, Forms,
  Dialogs, StdCtrls, Math;

type
  TMathOperation = class
  public
    function Execute(A, B: Double): Double; virtual; abstract;
  end;

  TMultiplicationOperation = class(TMathOperation)
  public
    function Execute(A, B: Double): Double; override;
  end;

  TAdditionOperation = class(TMathOperation)
  public
    function Execute(A, B: Double): Double; override;
  end;

  TDivisionOperation = class(TMathOperation)
  public
    function Execute(A, B: Double): Double; override;
  end;

  TSubtractionOperation = class(TMathOperation)
  public
    function Execute(A, B: Double): Double; override;
  end;

  TCalculatorForm = class(TForm)
    EditA: TEdit;
    EditB: TEdit;
    OutputMemo: TMemo;
    MultiplyButton: TButton;
    AddButton: TButton;
    DivideButton: TButton;
    SubtractButton: TButton;
    procedure MultiplyButtonClick(Sender: TObject);
    procedure AddButtonClick(Sender: TObject);
    procedure DivideButtonClick(Sender: TObject);
    procedure SubtractButtonClick(Sender: TObject);
  private
    { Private declarations }
    procedure PerformCalculation(Operation: TMathOperation);
  public
    { Public declarations }
  end;

var
  CalculatorForm: TCalculatorForm;

implementation

{$R *.dfm}

{ TMathOperation }

{ TMultiplicationOperation }

function TMultiplicationOperation.Execute(A, B: Double): Double;
begin
  Result := A * B;
end;

{ TAdditionOperation }

function TAdditionOperation.Execute(A, B: Double): Double;
begin
  Result := A + B;
end;

{ TDivisionOperation }

function TDivisionOperation.Execute(A, B: Double): Double;
begin
  if B <> 0 then
    Result := A / B
  else
    raise Exception.Create('Cannot divide by zero.');
end;

{ TSubtractionOperation }

function TSubtractionOperation.Execute(A, B: Double): Double;
begin
  Result := A - B;
end;

{ TCalculatorForm }

procedure TCalculatorForm.PerformCalculation(Operation: TMathOperation);
var
  A, B, Result: Double;
begin
  try
    A := StrToFloat(EditA.Text);
    B := StrToFloat(EditB.Text);
    Result := Operation.Execute(A, B);
    OutputMemo.Lines.Add(Format('Operation: %s %s %s = %s', [FloatToStr(A), Operation.ClassName, FloatToStr(B), FloatToStr(Result)]));
  except
    on E: Exception do
      OutputMemo.Lines.Add('Error: ' + E.Message);
  end;
end;

procedure TCalculatorForm.MultiplyButtonClick(Sender: TObject);
begin
  PerformCalculation(TMultiplicationOperation.Create);
end;

procedure TCalculatorForm.AddButtonClick(Sender: TObject);
begin
  PerformCalculation(TAdditionOperation.Create);
end;

procedure TCalculatorForm.DivideButtonClick(Sender: TObject);
begin
  PerformCalculation(TDivisionOperation.Create);
end;

procedure TCalculatorForm.SubtractButtonClick(Sender: TObject);
begin
  PerformCalculation(TSubtractionOperation.Create);
end;

end.
