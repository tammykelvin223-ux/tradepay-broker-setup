import React, { useState } from 'react';
import { ArrowLeft, Upload, Clock, CheckCircle, XCircle, Plus } from 'lucide-react';
import { Button } from '@/components/ui/button';
import { Card, CardContent, CardDescription, CardHeader, CardTitle } from '@/components/ui/card';
import { Input } from '@/components/ui/input';
import { Label } from '@/components/ui/label';
import { Select, SelectContent, SelectItem, SelectTrigger, SelectValue } from '@/components/ui/select';
import { Textarea } from '@/components/ui/textarea';
import { Badge } from '@/components/ui/badge';
import { Dialog, DialogContent, DialogDescription, DialogHeader, DialogTitle, DialogTrigger } from '@/components/ui/dialog';
import { toast } from 'sonner';

interface Transaction {
  id: string;
  type: 'deposit' | 'withdrawal';
  amount: number;
  status: 'pending' | 'approved' | 'rejected';
  date: string;
  paymentMethod: string;
  transactionId?: string;
  proofImage?: string;
  adminNote?: string;
}

const mockTransactions: Transaction[] = [
  {
    id: '1',
    type: 'deposit',
    amount: 25000,
    status: 'approved',
    date: '2024-01-15',
    paymentMethod: 'UPI',
    transactionId: 'UPI123456789',
    proofImage: '/api/placeholder/400/300'
  },
  {
    id: '2',
    type: 'deposit',
    amount: 15000,
    status: 'pending',
    date: '2024-01-20',
    paymentMethod: 'Bank Transfer',
    transactionId: 'BT987654321',
    proofImage: '/api/placeholder/400/300'
  }
];

export default function Transactions() {
  const [transactions, setTransactions] = useState<Transaction[]>(mockTransactions);
  const [isNewTransactionOpen, setIsNewTransactionOpen] = useState(false);
  const [newTransaction, setNewTransaction] = useState({
    amount: '',
    paymentMethod: '',
    transactionId: '',
    proofImage: null as File | null,
    notes: ''
  });

  const handleFileUpload = (event: React.ChangeEvent<HTMLInputElement>) => {
    const file = event.target.files?.[0];
    if (file) {
      setNewTransaction(prev => ({ ...prev, proofImage: file }));
    }
  };

  const handleSubmitTransaction = () => {
    if (!newTransaction.amount || !newTransaction.paymentMethod || !newTransaction.transactionId || !newTransaction.proofImage) {
      toast.error('Please fill all required fields and upload payment proof');
      return;
    }

    const transaction: Transaction = {
      id: Date.now().toString(),
      type: 'deposit',
      amount: parseFloat(newTransaction.amount),
      status: 'pending',
      date: new Date().toISOString().split('T')[0],
      paymentMethod: newTransaction.paymentMethod,
      transactionId: newTransaction.transactionId,
      proofImage: URL.createObjectURL(newTransaction.proofImage)
    };

    setTransactions(prev => [transaction, ...prev]);
    setNewTransaction({
      amount: '',
      paymentMethod: '',
      transactionId: '',
      proofImage: null,
      notes: ''
    });
    setIsNewTransactionOpen(false);
    toast.success('Transaction submitted successfully! Awaiting admin approval.');
  };

  const getStatusBadge = (status: string) => {
    switch (status) {
      case 'pending':
        return <Badge variant="secondary" className="bg-yellow-100 text-yellow-800"><Clock className="w-3 h-3 mr-1" />Pending</Badge>;
      case 'approved':
        return <Badge variant="secondary" className="bg-green-100 text-green-800"><CheckCircle className="w-3 h-3 mr-1" />Approved</Badge>;
      case 'rejected':
        return <Badge variant="secondary" className="bg-red-100 text-red-800"><XCircle className="w-3 h-3 mr-1" />Rejected</Badge>;
      default:
        return <Badge variant="secondary">{status}</Badge>;
    }
  };

  const formatCurrency = (amount: number) => {
    return new Intl.NumberFormat('en-IN', {
      style: 'currency',
      currency: 'INR'
    }).format(amount);
  };

  return (
    <div className="min-h-screen bg-gray-50">
      {/* Header */}
      <div className="bg-white border-b border-gray-200 px-4 py-4">
        <div className="flex items-center justify-between max-w-7xl mx-auto">
          <div className="flex items-center space-x-4">
            <Button variant="ghost" size="sm" onClick={() => window.history.back()}>
              <ArrowLeft className="w-4 h-4 mr-2" />
              Back to Dashboard
            </Button>
            <div>
              <h1 className="text-2xl font-bold text-gray-900">Transactions</h1>
              <p className="text-sm text-gray-600">Manage your deposits and withdrawals</p>
            </div>
          </div>
          <Dialog open={isNewTransactionOpen} onOpenChange={setIsNewTransactionOpen}>
            <DialogTrigger asChild>
              <Button className="bg-blue-600 hover:bg-blue-700">
                <Plus className="w-4 h-4 mr-2" />
                New Deposit
              </Button>
            </DialogTrigger>
            <DialogContent className="sm:max-w-md">
              <DialogHeader>
                <DialogTitle>New Deposit Request</DialogTitle>
                <DialogDescription>
                  Submit a new deposit request with payment proof for admin approval.
                </DialogDescription>
              </DialogHeader>
              <div className="space-y-4">
                <div>
                  <Label htmlFor="amount">Amount (INR) *</Label>
                  <Input
                    id="amount"
                    type="number"
                    placeholder="Enter amount"
                    value={newTransaction.amount}
                    onChange={(e) => setNewTransaction(prev => ({ ...prev, amount: e.target.value }))}
                  />
                </div>
                <div>
                  <Label htmlFor="paymentMethod">Payment Method *</Label>
                  <Select value={newTransaction.paymentMethod} onValueChange={(value) => setNewTransaction(prev => ({ ...prev, paymentMethod: value }))}>
                    <SelectTrigger>
                      <SelectValue placeholder="Select payment method" />
                    </SelectTrigger>
                    <SelectContent>
                      <SelectItem value="UPI">UPI</SelectItem>
                      <SelectItem value="Bank Transfer">Bank Transfer</SelectItem>
                      <SelectItem value="Net Banking">Net Banking</SelectItem>
                      <SelectItem value="Debit Card">Debit Card</SelectItem>
                    </SelectContent>
                  </Select>
                </div>
                <div>
                  <Label htmlFor="transactionId">Transaction ID *</Label>
                  <Input
                    id="transactionId"
                    placeholder="Enter transaction/reference ID"
                    value={newTransaction.transactionId}
                    onChange={(e) => setNewTransaction(prev => ({ ...prev, transactionId: e.target.value }))}
                  />
                </div>
                <div>
                  <Label htmlFor="proofImage">Payment Proof *</Label>
                  <div className="mt-1">
                    <Input
                      id="proofImage"
                      type="file"
                      accept="image/*"
                      onChange={handleFileUpload}
                      className="cursor-pointer"
                    />
                    <p className="text-xs text-gray-500 mt-1">Upload screenshot or receipt of payment</p>
                  </div>
                </div>
                <div>
                  <Label htmlFor="notes">Additional Notes (Optional)</Label>
                  <Textarea
                    id="notes"
                    placeholder="Any additional information..."
                    value={newTransaction.notes}
                    onChange={(e) => setNewTransaction(prev => ({ ...prev, notes: e.target.value }))}
                  />
                </div>
                <div className="flex space-x-2 pt-4">
                  <Button variant="outline" onClick={() => setIsNewTransactionOpen(false)} className="flex-1">
                    Cancel
                  </Button>
                  <Button onClick={handleSubmitTransaction} className="flex-1 bg-blue-600 hover:bg-blue-700">
                    Submit Request
                  </Button>
                </div>
              </div>
            </DialogContent>
          </Dialog>
        </div>
      </div>

      {/* Content */}
      <div className="max-w-7xl mx-auto px-4 py-6">
        {/* Balance Summary */}
        <div className="grid grid-cols-1 md:grid-cols-3 gap-6 mb-6">
          <Card>
            <CardHeader className="pb-2">
              <CardTitle className="text-sm font-medium text-gray-600">Available Balance</CardTitle>
            </CardHeader>
            <CardContent>
              <div className="text-2xl font-bold text-green-600">₹0.00</div>
              <p className="text-xs text-gray-500 mt-1">Ready for investment</p>
            </CardContent>
          </Card>
          <Card>
            <CardHeader className="pb-2">
              <CardTitle className="text-sm font-medium text-gray-600">Pending Deposits</CardTitle>
            </CardHeader>
            <CardContent>
              <div className="text-2xl font-bold text-yellow-600">
                {formatCurrency(transactions.filter(t => t.status === 'pending' && t.type === 'deposit').reduce((sum, t) => sum + t.amount, 0))}
              </div>
              <p className="text-xs text-gray-500 mt-1">Awaiting approval</p>
            </CardContent>
          </Card>
          <Card>
            <CardHeader className="pb-2">
              <CardTitle className="text-sm font-medium text-gray-600">Total Deposited</CardTitle>
            </CardHeader>
            <CardContent>
              <div className="text-2xl font-bold text-blue-600">
                {formatCurrency(transactions.filter(t => t.status === 'approved' && t.type === 'deposit').reduce((sum, t) => sum + t.amount, 0))}
              </div>
              <p className="text-xs text-gray-500 mt-1">Lifetime deposits</p>
            </CardContent>
          </Card>
        </div>

        {/* Transactions List */}
        <Card>
          <CardHeader>
            <CardTitle>Transaction History</CardTitle>
            <CardDescription>
              All your deposit and withdrawal transactions
            </CardDescription>
          </CardHeader>
          <CardContent>
            {transactions.length === 0 ? (
              <div className="text-center py-12">
                <Upload className="w-12 h-12 text-gray-400 mx-auto mb-4" />
                <h3 className="text-lg font-medium text-gray-900 mb-2">No transactions yet</h3>
                <p className="text-gray-500 mb-4">Make your first deposit to start investing</p>
                <Button onClick={() => setIsNewTransactionOpen(true)} className="bg-blue-600 hover:bg-blue-700">
                  <Plus className="w-4 h-4 mr-2" />
                  Make First Deposit
                </Button>
              </div>
            ) : (
              <div className="space-y-4">
                {transactions.map((transaction) => (
                  <div key={transaction.id} className="border border-gray-200 rounded-lg p-4">
                    <div className="flex items-center justify-between mb-3">
                      <div className="flex items-center space-x-3">
                        <div className={`w-10 h-10 rounded-full flex items-center justify-center ${
                          transaction.type === 'deposit' ? 'bg-green-100' : 'bg-red-100'
                        }`}>
                          {transaction.type === 'deposit' ? '↓' : '↑'}
                        </div>
                        <div>
                          <h4 className="font-medium text-gray-900 capitalize">{transaction.type}</h4>
                          <p className="text-sm text-gray-500">{transaction.date}</p>
                        </div>
                      </div>
                      <div className="text-right">
                        <div className="font-semibold text-lg">{formatCurrency(transaction.amount)}</div>
                        {getStatusBadge(transaction.status)}
                      </div>
                    </div>
                    <div className="grid grid-cols-1 md:grid-cols-2 gap-4 text-sm">
                      <div>
                        <span className="text-gray-500">Payment Method:</span>
                        <span className="ml-2 font-medium">{transaction.paymentMethod}</span>
                      </div>
                      <div>
                        <span className="text-gray-500">Transaction ID:</span>
                        <span className="ml-2 font-medium">{transaction.transactionId}</span>
                      </div>
                    </div>
                    {transaction.status === 'pending' && (
                      <div className="mt-3 p-3 bg-yellow-50 border border-yellow-200 rounded-md">
                        <p className="text-sm text-yellow-800">
                          <Clock className="w-4 h-4 inline mr-1" />
                          Your deposit is being reviewed by our admin team. You'll be notified once approved.
                        </p>
                      </div>
                    )}
                    {transaction.status === 'rejected' && transaction.adminNote && (
                      <div className="mt-3 p-3 bg-red-50 border border-red-200 rounded-md">
                        <p className="text-sm text-red-800">
                          <XCircle className="w-4 h-4 inline mr-1" />
                          Admin Note: {transaction.adminNote}
                        </p>
                      </div>
                    )}
                  </div>
                ))}
              </div>
            )}
          </CardContent>
        </Card>

        {/* Payment Instructions */}
        <Card className="mt-6">
          <CardHeader>
            <CardTitle>Payment Instructions</CardTitle>
          </CardHeader>
          <CardContent>
            <div className="space-y-4 text-sm">
              <div>
                <h4 className="font-medium mb-2">For UPI Payments:</h4>
                <p className="text-gray-600">Send payment to: <span className="font-mono bg-gray-100 px-2 py-1 rounded">admin@tradepay.upi</span></p>
              </div>
              <div>
                <h4 className="font-medium mb-2">For Bank Transfer:</h4>
                <div className="bg-gray-50 p-3 rounded-md">
                  <p><strong>Account Name:</strong> TradePay Global</p>
                  <p><strong>Account Number:</strong> 1234567890</p>
                  <p><strong>IFSC Code:</strong> HDFC0001234</p>
                  <p><strong>Bank:</strong> HDFC Bank</p>
                </div>
              </div>
              <div className="bg-blue-50 p-4 rounded-md">
                <h4 className="font-medium text-blue-900 mb-2">Important Notes:</h4>
                <ul className="text-blue-800 space-y-1 text-sm">
                  <li>• Always upload clear payment proof/screenshot</li>
                  <li>• Include correct transaction ID for faster processing</li>
                  <li>• Deposits are processed within 24 hours</li>
                  <li>• Contact support for any payment issues</li>
                </ul>
              </div>
            </div>
          </CardContent>
        </Card>
      </div>
    </div>
  );
}
